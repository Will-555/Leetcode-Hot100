[官方题解](https://leetcode.cn/problems/lru-cache/?envType=study-plan-v2&envId=top-100-liked)

```Java
class LRUCache {

    private int capacity;
    private LinkedHashMap<Integer, Integer> map;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new LinkedHashMap<Integer, Integer>(capacity, 0.75F, true) {
            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                return size() > capacity;
            }
        };
    }
    
    public int get(int key) {
        return map.getOrDefault(key, -1);
    }
    
    public void put(int key, int value) {
        map.put(key, value);
    }
}
```

```Java
class LRUCache {

    private Map<Integer, Node> map = new HashMap<>();
    int mCapacity;
    Node head, tail;

    public LRUCache(int capacity) {
        this.mCapacity = capacity;
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;        
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            remove(node);
            addNode(node);
            return node.value;
        }  else {
            return -1;
        }
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            remove(map.get(key));
        }
        if (map.size() == mCapacity) {
            remove(tail.prev);
        }
        addNode(new Node(key, value));
    }

    private void addNode(Node node) {
        map.put(node.key, node);
        Node headNext = head.next;
        head.next = node;
        node.prev = head;
        node.next = headNext;
        headNext.prev = node;
    }
    
    private void remove(Node node) {
        map.remove(node.key, node);
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    class Node {
        Node prev, next;
        int key, value;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
}
```
