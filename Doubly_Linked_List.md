
# Doubly Linked List

Its just kinda annoying to not have one of these when I need it.



```go
type Doubly_Linked_List[T any] struct {
    head, tail *DL_Node[T];
    count int;
}

type DL_Node[T any] struct {
    next, prev *DL_Node[T];
    item T;
}

func (dl *Doubly_Linked_List[T]) clear() {
    dl.head  = nil;
    dl.tail  = nil;
    dl.count = 0;
}

func (dl *Doubly_Linked_List[T]) add(new_item T) {
    dl.count += 1;

    new_node := new(DL_Node[T]);
    new_node.item = new_item;

    if dl.head == nil {
        // empty dl
        if dl.tail != nil { panic("what"); }
        dl.head = new_node;
        dl.tail = new_node;

    } else {
        new_node.prev = dl.tail;
        dl.tail.next = new_node;

        dl.tail = new_node;
    }
}

func (dl *Doubly_Linked_List[T]) remove(node *DL_Node[T]) (left_node, right_node *DL_Node[T]) {
    if node == nil { panic("why did you pass this to us."); }
    dl.count -= 1;

    if dl.head == node && dl.tail == node {
        dl.head = nil;
        dl.tail = nil;
        return nil, nil;
    }
    if dl.head == node {
        dl.head.next.prev = nil;
        dl.head = dl.head.next;
        return nil, dl.head;
    }
    if dl.tail == node {
        dl.tail.prev.next = nil;
        dl.tail = dl.tail.prev;
        return dl.tail, nil;
    }

    node.prev.next = node.next;
    node.next.prev = node.prev;
    return node.prev, node.next;
}
```
