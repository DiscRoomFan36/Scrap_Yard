
# Terminal Stuff

```c
// terminal escape codes, feels kinda hairy because its not wrapped in anything but strings
#define ESC     "\x1B"
#define CSI     ESC"["

#include <termios.h>

// returns true if it worked. sets errno on failure (on accident but it still works)
bool get_cursor_position(s32 *x, s32 *y) {
    struct termios term, restore;

    bool reset_terminal = false;
    bool result = true;

    ASSERT(STDIN_FILENO == 0);
    // get the current terminal settings, and a backup to restore to.
    if (tcgetattr(STDIN_FILENO, &term)    == -1) defer_return(false);
    Mem_Copy(&restore, &term, sizeof(struct termios));

    term.c_lflag &= ~(ICANON|ECHO); // clear these flags. "enable canonical mode" and "echo input charaters"
    // @paranoia - the standard says that this might not accept all off our changes? so we have to check with tcgettattr again
    if (tcsetattr(STDIN_FILENO, TCSANOW, &term) == -1) defer_return(false);
    reset_terminal = true;

    ASSERT(STDOUT_FILENO == 1);
    #define CURSOR_POSITION_SEQUENCE CSI"6n"
    write(STDOUT_FILENO, CURSOR_POSITION_SEQUENCE, strlen(CURSOR_POSITION_SEQUENCE));

    int matches = fscanf(stdin, CSI"%d;%dR", y, x);
    if (matches != 2) defer_return(false);

defer:

    if (reset_terminal) tcsetattr(STDIN_FILENO, TCSANOW, &restore);
    return result;
}
```


```c
#include <sys/ioctl.h>
// returns true if it worked. sets errno on failure
bool get_terminal_size(s32 *width, s32 *height) {
    struct winsize w;
    if (ioctl(STDOUT_FILENO, TIOCGWINSZ, &w) == -1) return false;

    *width  = w.ws_col; *height = w.ws_row;
    return true;
}
```


```c
void move_cursor_to(s32 x, s32 y) {
#define MAX_CONSOLE_SIZE_FOR_SANITY    512 // why would it be any bigger than this?
    ASSERT(0 <= x && x < MAX_CONSOLE_SIZE_FOR_SANITY);
    ASSERT(0 <= y && y < MAX_CONSOLE_SIZE_FOR_SANITY);
    fprintf(stdout, CSI"%d;%dH", y, x); // this shouldn't buffer right?
}
```


```c
int main(void) {
    s32 x, y;
    if (!get_cursor_position(&x, &y)) handle_error("Could not get cursor position");
    printf("x: %d, y: %d\n", x, y);

    s32 width, height;
    if (!get_terminal_size(&width, &height)) handle_error("Could not get terminal size");
    printf("width: %d, height: %d\n", width, height);

    s32 dx = 1;
    s32 dy = 1;

    for (int i = 0; i < 1000; i++) {
        if (x == 1)         dx =  1;
        if (x == width)     dx = -1;

        if (y == 1)         dy =  1;
        if (y == height)    dy = -1;

        x += dx; y += dy;

        move_cursor_to(x, y);
        printf("X"); fflush(stdout);

        usleep(10 * MILISECONDS_PER_MICROSECOND);
    }

    move_cursor_to(1, height);

    exit(0);
}
```

