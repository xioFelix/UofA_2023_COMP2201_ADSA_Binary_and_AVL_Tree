#include <iostream>
#include <sstream>
#include <vector>

// Node structure for AVL Tree
struct Node {
  int value;
  Node* left;
  Node* right;
  int height;

  Node(int v) : value(v), left(nullptr), right(nullptr), height(1) {}
};

class AVLTree {
 private:
  Node* root;

  // Utility functions
  int height(Node* N) { return (N == nullptr) ? 0 : N->height; }

  int getBalance(Node* N) {
    return (N == nullptr) ? 0 : height(N->left) - height(N->right);
  }

  Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;

    // Perform rotation
    y->left = x;
    x->right = T2;

    // Update heights
    x->height = std::max(height(x->left), height(x->right)) + 1;
    y->height = std::max(height(y->left), height(y->right)) + 1;

    // Return new root
    return y;
  }

  Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;

    // Perform rotation
    x->right = y;
    y->left = T2;

    // Update heights
    y->height = std::max(height(y->left), height(y->right)) + 1;
    x->height = std::max(height(x->left), height(x->right)) + 1;

    // Return new root
    return x;
  }

  Node* insert(Node* node, int value) {
    if (node == nullptr) return new Node(value);

    // 1. Perform the normal BST insertion
    if (value < node->value)
      node->left = insert(node->left, value);
    else if (value > node->value)
      node->right = insert(node->right, value);
    else
      return node;

    // Update height
    node->height = 1 + std::max(height(node->left), height(node->right));

    // Get balance factor
    int balance = getBalance(node);

    // If unbalanced, perform rotations:
    // Left Left Case
    if (balance > 1 && value < node->left->value) return rightRotate(node);

    // Right Right Case
    if (balance < -1 && value > node->right->value) return leftRotate(node);

    // Left Right Case
    if (balance > 1 && value > node->left->value) {
      node->left = leftRotate(node->left);
      return rightRotate(node);
    }

    // Right Left Case
    if (balance < -1 && value < node->right->value) {
      node->right = rightRotate(node->right);
      return leftRotate(node);
    }

    return node;
  }

  Node* minValueNode(Node* node) {
    Node* current = node;
    while (current->right != nullptr) current = current->right;
    return current;
  }

  Node* deleteNode(Node* root, int value) {
    if (!root) return root;

    // 1. Perform the standard BST delete
    if (value < root->value)
      root->left = deleteNode(root->left, value);
    else if (value > root->value)
      root->right = deleteNode(root->right, value);
    else {
      if ((root->left == NULL) || (root->right == NULL)) {
        Node* temp = root->left ? root->left : root->right;
        if (temp==NULL) {
          temp = root;
          root = NULL;
        } else {
          *root = *temp;
        }
        free(temp);
      } else {
        // Change
        Node* temp = minValueNode(root->left);
        root->value = temp->value;
        root->left = deleteNode(root->left, temp->value);
      }
    }

    if (!root) return root;

    // 2. Update height
    root->height = 1 + std::max(height(root->left), height(root->right));

    // 3. Get balance factor
    int balance = getBalance(root);
    int left_balance = getBalance(root->left);
    int right_balance = getBalance(root->right);

    // 4. If unbalanced, perform rotations:
    // Left Left Case
    if (balance > 1 && left_balance >= 0) return rightRotate(root);

    // Left Right Case
    if (balance > 1 && left_balance < 0) {
      root->left = leftRotate(root->left);
      return rightRotate(root);
    }

    // Right Right Case
    if (balance < -1 && right_balance <= 0) return leftRotate(root);

    // Right Left Case
    if (balance < -1 && right_balance > 0) {
      root->right = rightRotate(root->right);
      return leftRotate(root);
    }

    return root;
  }

  void preOrder(Node* root, std::vector<int>& output) {
    if (root != nullptr) {
      output.push_back(root->value);
      preOrder(root->left, output);
      preOrder(root->right, output);
    }
  }

  void inOrder(Node* root, std::vector<int>& output) {
    if (root != nullptr) {
      inOrder(root->left, output);
      output.push_back(root->value);
      inOrder(root->right, output);
    }
  }

  void postOrder(Node* root, std::vector<int>& output) {
    if (root != nullptr) {
      postOrder(root->left, output);
      postOrder(root->right, output);
      output.push_back(root->value);
    }
  }

 public:
  AVLTree() : root(nullptr) {}

  void insert(int value) { root = insert(root, value); }

  void remove(int value) { root = deleteNode(root, value); }

  std::vector<int> preOrder() {
    std::vector<int> output;
    preOrder(root, output);
    return output;
  }

  std::vector<int> inOrder() {
    std::vector<int> output;
    inOrder(root, output);
    return output;
  }

  std::vector<int> postOrder() {
    std::vector<int> output;
    postOrder(root, output);
    return output;
  }
  /*
// Print tree to debug
  void printLevel(Node* root, int level) {
    if (root == nullptr) {
      if (level == 1) std::cout << "NULL ";
      return;
    }
    if (level == 1) {
      std::cout << root->value << " ";
    } else if (level > 1) {
      printLevel(root->left, level - 1);
      printLevel(root->right, level - 1);
    }
  }

  void printTree() {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
      printLevel(root, i);
      std::cout << std::endl;
    }
  }
//
*/
};

  int main() {
    AVLTree avl;
    std::string input;
    std::getline(std::cin, input);

    std::stringstream ss(input);
    std::string token;
    while (ss >> token) {
      if (token[0] == 'A') {
        avl.insert(std::stoi(token.substr(1)));
        /*
            // Print tree to debug
            avl.printTree();
        std::cout << "-------------" << std::endl;
        //
        */
      } else if (token[0] == 'D') {
        avl.remove(std::stoi(token.substr(1)));
        /*
          // Print tree to debug
          avl.printTree();
          std::cout << "-------------" << std::endl;
          //
          */
      } else {
        std::vector<int> result;
        if (token == "PRE")
          result = avl.preOrder();
        else if (token == "IN")
          result = avl.inOrder();
        else if (token == "POST")
          result = avl.postOrder();

        if (result.empty()) {
          std::cout << "EMPTY";
        } else {
          for (int val : result) {
            std::cout << val << " ";
          }
        }
      }
    }
    return 0;
  }