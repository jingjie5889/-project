# -project
mport time
from graphviz import Graph, Digraph
import random
class Node:
    def __init__(self, key=None, next=None):
        self.key = key
        self.next = next

class CompleteBinaryTree:
    def __init__(self):
        self.root = None

    def get_parent(self, i):
        if i == 0:
            return None
        return (i - 1) // 2

    def get_left_child(self, i):
        return 2 * i + 1

    def get_right_child(self, i):
        return 2 * i + 2

    def get_size(self):
        # returns the number of elements in the tree
        if self.root is None:
            return 0

        size = 1
        curr = self.root
        while curr.next is not None:
            size += 1
            curr = curr.next
        return size

    def get_node(self, i):
        # returns the node at the given index
        curr = self.root
        for j in range(i):
            curr = curr.next
        return curr

    def swap(self, i, j):
        # swaps the keys of the nodes at the given indices
        node_i = self.get_node(i)
        node_j = self.get_node(j)
        node_i.key, node_j.key = node_j.key, node_i.key

    def insert(self, key):
        # create a new node and insert it at the end of the list
        new_node = Node(key)
        if self.root is None:
            self.root = new_node
            return

        curr = self.root
        while curr.next is not None:
            curr = curr.next

        curr.next = new_node

        # percolate up to maintain heap property
        i = self.get_size() - 1
        parent = self.get_parent(i)
        while parent is not None and self.get_node(parent).key > self.get_node(i).key:
            self.swap(parent, i)
            i = parent
            parent = self.get_parent(i)

    def delMin(self):
        # return the root (minimum element) and remove it from the list
        min_node = self.root
        if self.root.next is None:
            self.root = None
            return min_node.key

        prev = None
        curr = self.root
        while curr.next is not None:
            prev = curr
            curr = curr.next

        prev.next = None

        # percolate down to maintain heap property
        self.root.key = curr.key
        i = 0
        while True:
            left_child = self.get_left_child(i)
            right_child = self.get_right_child(i)
            if left_child >= self.get_size():
                break
            if right_child >= self.get_size() or self.get_node(left_child).key < self.get_node(right_child).key:
                min_child = left_child
            else:
                min_child = right_child
            if self.get_node(i).key > self.get_node(min_child).key:
                self.swap(i, min_child)
                i = min_child
            else:
                break
        return min_node.key

    def benchmark_heap(heap):
        # measure the running time of the insert and delMin methods
        input_sizes = [10, 100, 1000, 10000, 100000]
        insert_times = []
        del_min_times = []
        for n in input_sizes:
            keys = list(range(n))
            random.shuffle(keys)

            start = time.time()
            for key in keys:
                heap.insert(key)
            end = time.time()
            insert_times.append(end - start)

            start = time.time()
            for _ in range(n):
                heap.delMin()
            end = time.time()
            del_min_times.append(end - start)

        return insert_times, del_min_times

    def visualize_performance(insert_times, del_min_times):
        # visualize the running times of the insert and delMin methods
        plt.plot(input_sizes, insert_times, label='insert')
        plt.plot(input_sizes, del_min_times, label='delMin')
        plt.xlabel('input size')
        plt.ylabel('running time (seconds)')
        plt.legend()
        plt.show()

# test the implementation
heap = CompleteBinaryTree()
visualize_heap(heap)
insert_times, del_min_times = benchmark_heap(heap)
visualize_performance(insert_times, del_min_times)










1.	The CompleteBinaryTree ADT is based on a singly linked list representation of a complete binary tree. The first element's index is assigned as 0. Three operations are implemented to get the parent, left child, and right child of a node at a given index, respectively. These operations are get_parent(self, i), get_left_child(self, i), and get_right_child(self, i).

2.	The minimum priority queue is implemented using the CompleteBinaryTree ADT. The insert(self, key) method adds a new node with the given key to the end of the list and then uses a percolate-up operation to maintain the heap property. The delMin(self) method removes the root (minimum element) of the heap and returns it. It then replaces the root with the last element in the list and performs a percolate-down operation to maintain the heap property.


3.	The insert(self, key) method has a time complexity of O(log n) because it involves a percolate-up operation that compares the key of the new node with its parent at each level of the tree, and the tree has a maximum height of log n. The delMin(self) method has a time complexity of O(log n) because it involves a percolate-down operation that compares the key of the root with its children at each level of the tree, and the tree has a maximum height of log n.

4.	A simple performance benchmark is conducted by measuring the running time of the insert(self, key) and delMin(self) methods for various input sizes. The results are then visualized using a line plot.


5.	The corresponding tree structure of the linked list based heap can be drawn using the visualize_heap(heap) function, which uses the graphviz library to generate a graphical representation of the tree.




import time
from graphviz import Graph, Digraph
import random
class Node:
    def __init__(self, key=None, next=None):
        self.key = key
        self.next = next

class CompleteBinaryTree:
    def __init__(self):
        self.root = None

    def get_parent(self, i):
        if i == 0:
            return None
        return (i - 1) // 2

    def get_left_child(self, i):
        return 2 * i + 1

    def get_right_child(self, i):
        return 2 * i + 2

    def get_size(self):
        # returns the number of elements in the tree
        if self.root is None:
            return 0

        size = 1
        curr = self.root
        while curr.next is not None:
            size += 1
            curr = curr.next
        return size

    def get_node(self, i):
        # returns the node at the given index
        curr = self.root
        for j in range(i):
            curr = curr.next
        return curr

    def swap(self, i, j):
        # swaps the keys of the nodes at the given indices
        node_i = self.get_node(i)
        node_j = self.get_node(j)
        node_i.key, node_j.key = node_j.key, node_i.key

    def insert(self, key):
        # create a new node and insert it at the end of the list
        new_node = Node(key)
        if self.root is None:
            self.root = new_node
            return

        curr = self.root
        while curr.next is not None:
            curr = curr.next

        curr.next = new_node

        # percolate up to maintain heap property
        i = self.get_size() - 1
        parent = self.get_parent(i)
        while parent is not None and self.get_node(parent).key > self.get_node(i).key:
            self.swap(parent, i)
            i = parent
            parent = self.get_parent(i)

    def delMin(self):
        # return the root (minimum element) and remove it from the list
        min_node = self.root
        if self.root.next is None:
            self.root = None
            return min_node.key

        prev = None
        curr = self.root
        while curr.next is not None:
            prev = curr
            curr = curr.next

        prev.next = None

        # percolate down to maintain heap property
        self.root.key = curr.key
        i = 0
        while True:
            left_child = self.get_left_child(i)
            right_child = self.get_right_child(i)
            if left_child >= self.get_size():
                break
            if right_child >= self.get_size() or self.get_node(left_child).key < self.get_node(right_child).key:
                min_child = left_child
            else:
                min_child = right_child
            if self.get_node(i).key > self.get_node(min_child).key:
                self.swap(i, min_child)
                i = min_child
            else:
                break
        return min_node.key

    def benchmark_heap(heap):
        # measure the running time of the insert and delMin methods
        input_sizes = [10, 100, 1000, 10000, 100000]
        insert_times = []
        del_min_times = []
        for n in input_sizes:
            keys = list(range(n))
            random.shuffle(keys)

            start = time.time()
            for key in keys:
                heap.insert(key)
            end = time.time()
            insert_times.append(end - start)

            start = time.time()
            for _ in range(n):
                heap.delMin()
            end = time.time()
            del_min_times.append(end - start)

        return insert_times, del_min_times

    def visualize_performance(insert_times, del_min_times):
        # visualize the running times of the insert and delMin methods
        plt.plot(input_sizes, insert_times, label='insert')
        plt.plot(input_sizes, del_min_times, label='delMin')
        plt.xlabel('input size')
        plt.ylabel('running time (seconds)')
        plt.legend()
        plt.show()

# test the implementation
heap = CompleteBinaryTree()
visualize_heap(heap)
insert_times, del_min_times = benchmark_heap(heap)
visualize_performance(insert_times, del_min_times)
