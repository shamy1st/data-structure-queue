# Queue

![](https://github.com/shamy1st/data-structure/blob/main/images/queue.png)

--                    | enqueue     | dequeue     | peek | isEmpty | isFull
----------------------|-------------|-------------|------|---------|-------
Queue                 | O(1)        | O(1)        | O(1) | O(1)    | O(1)
PriorityQueue (Array) | O(n)        | O(1)        | O(1) | O(1)    | O(1)
PriorityQueue (Heap)  | O(log n)    | O(log n)    | O(1) | O(1)    | O(1)

* **Queue Interface**
  * **with ArrayDeque** (Double Ended Queue)
  * **with LinkedList** 

        public class Main {
            public static void main(String[] args) {
                Queue<Integer> queue = new ArrayDeque<>();

                queue.add(10); //enqueue item, if queue is full throw exception
                queue.offer(20); //enqueue item, doesn't throw exception

                queue.remove(); //dequeue item, if queue is empty throw exception
                queue.poll(); //dequeue item, doesn't throw exception

                queue.element(); //return front item, if queue is empty throw exception
                queue.peek(); //return front item, if queue is empty return null
            }
        }

* **Reverse a Queue** (using Stack)

        public class Main {
            public static void main(String[] args) {
                Queue<Integer> queue = new ArrayDeque<>();
                queue.add(10);
                queue.add(20);
                queue.add(30);
                System.out.println(queue);
                System.out.println(reverse(queue));
            }

            public static Queue<Integer> reverse(Queue<Integer> queue) {
                Stack<Integer> stack = new Stack<>();
                while(!queue.isEmpty())
                    stack.push(queue.remove());

                while(!stack.isEmpty())
                    queue.add(stack.pop());

                return queue;
            }
        }

* **Queue Implementation**
  * **with Array**
  
        public class Main {
            public static void main(String[] args) {
                Queue<Integer> queue = new Queue<>(5);
                queue.enqueue(10);
                queue.enqueue(20);
                queue.enqueue(30);
                queue.dequeue();
                System.out.println(queue);
            }
        }

        public class Queue<T> {
            private T[] items;
            private int front;
            private int rear;
            private int count;

            public Queue(int capacity) {
                items = (T[]) new Object[capacity];
            }

            public void enqueue(T item) {
                if(count == items.length)
                    throw new IllegalStateException();
                items[rear] = item;
                rear = (rear + 1) % items.length;
                count++;
            }

            public T dequeue() {
                if(count == 0)
                    throw new NullPointerException();
                count--;
                T item = items[front];
                items[front] = null;
                front = (front + 1) % items.length;
                return item;
            }

            public boolean isEmpty() {
                return count == 0;
            }

            public boolean isFull() {
                return count == items.length;
            }

            @Override
            public String toString() {
                return "Queue{" + Arrays.toString(items) + "}";
            }
        }
  
  * **with LinkedList**
  
  * **with Stack**
  
        public class Main {
            public static void main(String[] args) {
                Queue<Integer> queue = new Queue<>();
                queue.enqueue(10);
                queue.enqueue(20);
                queue.enqueue(30);
                System.out.println(queue.dequeue());
            }
        }

        public class Queue<T> {
            private Stack<T> enqueueStack;
            private Stack<T> dequeueStack;

            public Queue() {
                enqueueStack = new Stack<>();
                dequeueStack = new Stack<>();
            }

            //O(1)
            public void enqueue(T item) {
                enqueueStack.push(item);
            }

            //O(n)
            public T dequeue() {
                if (isEmpty())
                    throw new IllegalStateException();

                if(dequeueStack.isEmpty()) {
                    while (!enqueueStack.isEmpty())
                        dequeueStack.push(enqueueStack.pop());
                }
                return dequeueStack.pop();
            }

            public boolean isEmpty() {
                return enqueueStack.isEmpty() && dequeueStack.isEmpty();
            }
        }
  
* **Priority Queue**: items are sorted

  * **with Array**
  
        public class Main {
            public static void main(String[] args) {
                PriorityQueue queue = new PriorityQueue(5);
                queue.add(5);
                queue.add(3);
                queue.add(1);
                queue.add(2);
                queue.add(4);
                System.out.println(queue.remove());
                System.out.println(queue.remove());
                System.out.println(queue);
            }
        }

        public class PriorityQueue {
            private int[] items;
            private int count;

            public PriorityQueue(int capacity) {
                items = new int[capacity];
            }

            //O(n)
            public void add(int item) {
                if(isFull())
                    throw new IllegalStateException();

                //O(n)
                int index = shiftItemsToInsert(item);
                items[index] = item;
                count++;
            }

            //O(1)
            public int remove() {
                if(isEmpty())
                    throw new NullPointerException();

                int item = items[count-1];
                items[--count] = 0;
                return item;
            }

            public boolean isEmpty() {
                return count == 0;
            }

            public boolean isFull() {
                return count == items.length;
            }

            @Override
            public String toString() {
                return "PriorityQueue{" + Arrays.toString(items) + "}";
            }

            private int shiftItemsToInsert(int item) {
                int index;
                for(index=count-1;index>=0;index--) {
                    items[index+1] = items[index];
                    if(items[index] < item) {
                        break;
                    }
                }
                return index+1;
            }
        }
  
  * **with Heap**
  
        public class PriorityQueue<T extends Number & Comparable<T>> {
            private Heap<T> heap;

            public PriorityQueue(int capacity) {
                heap = new Heap<T>(capacity);
            }

            public void enqueue(T item) {
                heap.insert(item);
            }

            public T dequeue() {
                return heap.remove();
            }

            public boolean isEmpty() {
                return heap.isEmpty();
            }

            public boolean isFull() {
                return heap.isFull();
            }
        }
