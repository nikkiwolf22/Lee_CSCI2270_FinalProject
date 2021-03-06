# Methods In Library

## addItemNode

**Function prototype**:

void ItemTree::addItemNode(string item, int price)


**Function description**:

This method adds items into the binary search tree. 


**Example**:

ItemTree Item;

Item.addItemNode(itemAdd, price)


**Precondition**: Unused locations in hashTable vector are NULL. Movies stored using correctly calculated hash code calculated from movie title. The input string name is movie title.

**Post condition**: movie node deleted from chain and memory freed. Pointers updated to bypass deleted movie.


```javascript
void ItemTree::addItemNode(string item, int price)
{
    ItemNode *next = new ItemNode(item, price);
    ItemNode *x = root;
    ItemNode *P = NULL;

    if (x == NULL)
    {
        root = next;
        root->parent = NULL;
        root->leftChild = NULL;
        root->rightChild = NULL;
    }
    else
    {
        while (x != NULL)
        {
            P = x;
            if (item.compare(x->item) < 0)
            {
                x = x->leftChild;
            }
            else
            {
                x = x->rightChild;
            }
        }

        next->parent = P;
        next->leftChild = NULL;
        next->rightChild = NULL;
        if (item.compare(P->item) < 0)
        {
            P->leftChild = next;
        }
        else
        {
            P->rightChild = next;
        }
    }
}
```


## deleteItemNode

**Function prototype**:
void ItemTree::deleteItemNode(string item);

**Function description**:
This method deletes the node that the item information is stored in. 

**Example**:
ItemTree Item;
Item.addItemNode(itemAdd, price);

**Precondition**:
There has to be at least one item pre-existing in the list.
User must type the item with the same spelling and capitalization as they did when they added the item or how the item appears in the printed list.

**Post condition**:
The item the user have removed  is removed and will disappear from the print inventory list.

```javascript
void ItemTree::deleteItemNode(string item)
{
    ItemNode *nodeDelete = searchItemTree(root, item);

    if (nodeDelete == NULL)
    {
        cout << "Item not found." << endl;
    }
    else
    {
        soldList.push_back(nodeDelete->item);

        if (nodeDelete->leftChild==NULL || nodeDelete->rightChild==NULL)
        {
            // No Children
            if (nodeDelete->leftChild==NULL && nodeDelete->rightChild==NULL)
            {
                if (nodeDelete == nodeDelete->parent->leftChild)
                {
                    nodeDelete->parent->leftChild = NULL;
                }
                else if (nodeDelete == nodeDelete->parent->rightChild)
                {
                    nodeDelete->parent->rightChild = NULL;
                }

                delete nodeDelete;
            }
            // One child
            else
            {
                if (nodeDelete->leftChild == NULL)
                {
                    if (nodeDelete == nodeDelete->parent->leftChild)
                    {
                        nodeDelete->parent->leftChild = nodeDelete->rightChild;
                    }
                    else if (nodeDelete == nodeDelete->parent->rightChild)
                    {
                        nodeDelete->parent->rightChild = nodeDelete->rightChild;
                    }
                }
                else if (nodeDelete->rightChild == NULL)
                {
                    if (nodeDelete == nodeDelete->parent->leftChild)
                    {
                        nodeDelete->parent->leftChild = nodeDelete->leftChild;
                    }
                    else if (nodeDelete == nodeDelete->parent->rightChild)
                    {
                        nodeDelete->parent->rightChild = nodeDelete->leftChild;
                    }
                }

                delete nodeDelete;
            }
        }
        // Two children
        else if (nodeDelete->leftChild!=NULL && nodeDelete->rightChild!=NULL)
        {
            ItemNode *replNode = minimumNode(nodeDelete->rightChild);
            nodeDelete->item = replNode->item;
            nodeDelete->price = replNode->price;

            if (replNode->rightChild != NULL)
            {
                replNode->rightChild->parent = replNode->parent;
            }
            if (replNode == replNode->parent->leftChild)
            {
                replNode->parent->leftChild = replNode->rightChild;
            }
            else if (replNode == replNode->parent->rightChild)
            {
                replNode->parent->rightChild = replNode->rightChild;
            }

            delete replNode;
        }
    }
}
```


## findItem

**Function prototype**:
void ItemTree::findItem(string item);

**Function description**:
This method searches through the BST tree and finds the item the user is searching for.


**Example**:
ItemTree Item;
Item.addItemNode(itemAdd, price)

**Precondition**:
The spelling and capitalization must be the same as it shows in the print inventory list.
If the item you entered does not exist, the program will say 'Item not found'.

**Post condition**:
If the item the user is searching for exist in the inventory list, the program will print the item name and price.

```javascript
void ItemTree::findItem(string item)
{
    if (searchItemTree(root, item) == NULL)
    {
        cout << "Item not found" << endl;
    }
    else
    {
        cout << "Item Information:" << endl
             << "******************" << endl
             << "Item:" << searchItemTree(root, item)->item << endl
             << "Price:" << searchItemTree(root, item)->price << endl;
    }
}
```


## searchItemNode

**Function prototype**:
ItemNode* ItemTree::searchItemTree(ItemNode *node, string item);

**Function description**:
This method searches through the BST for the specific node.


**Example**:
Implemented in findItem(item):
searchItemTree(root, item);

**Precondition**:
Item has to exist. If the item does not exist in the inventory list, it will print 'Item not found'.

**Post condition**:
If conditions are satified, it will be considered as found and the item and price will be printed through void ItemTree::findItem(string item);.

```javascript
ItemNode* ItemTree::searchItemTree(ItemNode *node, string item)
{
    if (node == NULL)
    {
        return node;
    }
    else if (node->item == item)
    {
        return node;
    }
    else
    {
        if (node->item.compare(item) <= 0)
        {
            return searchItemTree(node->rightChild, item);
        }
        else
        {
            return searchItemTree(node->leftChild, item);
        }
    }
}
```


## countItemNodes

**Function prototype**:
int ItemTree::countItemNodes(ItemNode *node);

**Function description**:
This method counts how many items are in the inventory list.

**Example**:
Item.countItemNodes();

**Precondition**:
has to have items in inventory list

**Post condition**:
Post condition: prints number of items remain

```javascript
int ItemTree::countItemNodes(ItemNode *node)
{
    if (node->leftChild != NULL)
    {
        countItemNodes(node->leftChild);
    }

    cnt++;

    if (node->rightChild != NULL)
    {
        countItemNodes(node->rightChild);
    }
}
```


## printInOrder

**Function prototype**:
void ItemTree::printInOrder(ItemNode *node);

**Function description**:
This method print the values in the tree in sorted order: left-parent-right.

**Example**:
ItemTree Item;
Item.printInOrder();

**Precondition**:
must have BST

**Post condition**:
prints items out in left-parent-right order.

```javascript
void ItemTree::printInOrder(ItemNode *node)
{
    if (node->leftChild != NULL)
    {
        printInOrder(node->leftChild);
    }

    cout << node->item << ":" << node->price << endl;

    if (node->rightChild != NULL)
    {
        printInOrder(node->rightChild);
    }
}
```



## printPreOrder

**Function prototype**:
void ItemTree::printPreOrder(ItemNode *node);

**Function description**:
This method print the root value before printing the left and right child values.

**Example**:
ItemTree Item;
Item.printPreOrder();

**Precondition**:
 must have BST
 
**Post condition**:
Post condition: prints items out in root then left and right child order.

```javascript
void ItemTree::printPreOrder(ItemNode *node)
{
    cout << node->item << ":" << node->price << endl;

    if (node->leftChild != NULL)
    {
        printPreOrder(node->leftChild);
    }

    if (node->rightChild != NULL)
    {
        printPreOrder(node->rightChild);
    }
}
```



## printPostOrder

**Function prototype**:
void ItemTree::printPostOrder(ItemNode *node)

**Function description**:
This function print the left and right child values before the parent value.

**Example**:
ItemTree Item;
printPostOrder()
{
    printPostOrder(root);
}

**Precondition**:
must have BST

**Post condition**:
prints items out in left and right child value first then parent order.

```javascript
void ItemTree::printPostOrder(ItemNode *node)
{
    if (node->leftChild != NULL)
    {
        printPostOrder(node->leftChild);
    }

    if (node->rightChild != NULL)
    {
        printPostOrder(node->rightChild);
    }

    cout << node->item << ":" << node->price << endl;
}
```


## printSold

**Function prototype**:
void ItemTree::printSold()

**Function description**:
This method prints how many items have been sold. Collects deleted items and prints each item through a for loop.

**Example**:
ItemTree Item;
Item.printSold()

**Precondition**:
items but have been 'deleted' from the list, marking it 'sold'.

**Post condition**:
lists all the deleted items(sold items).

```javascript
void ItemTree::printSold()
{
    if (soldList.size() == 0)
    {
        cout << "List of sold items unavailable" << endl;
    }
    else
    {
        cout << "<ITEM>" << endl;

        for (int i=0; i<soldList.size(); i++)
        {
            cout << soldList[i] << endl;
        }
    }
}
```
