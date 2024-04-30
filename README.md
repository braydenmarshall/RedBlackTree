public class Starter {
    public static void main(String[] args) {
        RedBlackTree redBlackTree = new RedBlackTree();

        // Insert values
        redBlackTree.insert(10);
        redBlackTree.insert(5);
        redBlackTree.insert(15);
        redBlackTree.insert(3);
        redBlackTree.insert(7);

        // Test Find
        System.out.println("Find 7: " + redBlackTree.find(7)); // Should print true
        System.out.println("Find 20: " + redBlackTree.find(20)); // Should print false

        // Test toString
        System.out.println("Sorted order: " + redBlackTree.toString()); // Should print 3 5 7 10 15
        
    }
}

class RedBlackTree {
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    private class Node {
        int value;
        Node left, right;
        boolean color;

        Node(int value, boolean color) {
            this.value = value;
            this.color = color;
        }
    }

    private Node root;

    // Public methods

    void insert(int value) {
        root = insert(root, value);
        root.color = BLACK;
    }

    boolean find(int value) {
        return find(root, value);
    }

    public String toString() {
        StringBuilder result = new StringBuilder();
        inorder(root, result);
        return result.toString().trim();
    }

    // Private helper methods

    private Node insert(Node node, int value) {
        if (node == null)
            return new Node(value, RED);

        if (value < node.value)
            node.left = insert(node.left, value);
        else if (value > node.value)
            node.right = insert(node.right, value);

        if (isRed(node.right) && !isRed(node.left))
            node = rotateLeft(node);
        if (isRed(node.left) && isRed(node.left.left))
            node = rotateRight(node);
        if (isRed(node.left) && isRed(node.right))
            flipColors(node);

        return node;
    }

    private boolean find(Node node, int value) {
        while (node != null) {
            if (value < node.value)
                node = node.left;
            else if (value > node.value)
                node = node.right;
            else
                return true;
        }
        return false;
    }

    private void inorder(Node node, StringBuilder result) {
        if (node != null) {
            inorder(node.left, result);
            result.append(node.value).append(" ");
            inorder(node.right, result);
        }
    }

    private boolean isRed(Node node) {
        return node != null && node.color == RED;
    }

    private Node rotateLeft(Node h) {
        Node x = h.right;
        h.right = x.left;
        x.left = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }

    private Node rotateRight(Node h) {
        Node x = h.left;
        h.left = x.right;
        x.right = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }

    private void flipColors(Node h) {
        h.color = RED;
        h.left.color = BLACK;
        h.right.color = BLACK;
    }
}

import org.junit.Test;
import static org.junit.Assert.*;

public class RedBlackTreeTest {

    @Test
    public void testInsert() {
        RedBlackTree redBlackTree = new RedBlackTree();

        redBlackTree.insert(10);
        redBlackTree.insert(5);
        redBlackTree.insert(15);
        redBlackTree.insert(3);
        redBlackTree.insert(7);

        assertEquals("3 5 7 10 15", redBlackTree.toString().trim());
    }

    @Test
    public void testFind() {
        RedBlackTree redBlackTree = new RedBlackTree();

        redBlackTree.insert(10);
        redBlackTree.insert(5);
        redBlackTree.insert(15);

        assertTrue(redBlackTree.find(10));
        assertTrue(redBlackTree.find(5));
        assertTrue(redBlackTree.find(15));
        assertFalse(redBlackTree.find(3));
    }

    @Test
    public void testDelete() {
        RedBlackTree redBlackTree = new RedBlackTree();

        redBlackTree.insert(10);
        redBlackTree.insert(5);
        redBlackTree.insert(15);

        assertTrue(redBlackTree.find(10));
        assertTrue(redBlackTree.find(5));
        assertTrue(redBlackTree.find(15));

        assertFalse(redBlackTree.find(10));
        assertTrue(redBlackTree.find(5));
        assertTrue(redBlackTree.find(15));
    }
}
