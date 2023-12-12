#include <stdint.h>
#include <stdbool.h>
#include <stdlib.h>
#include <assert.h>

//--------------------------------------------------
// List of Nodes
//--------------------------------------------------
struct NodeImpl {
    struct NodeImpl* next;
    Data_t data;
};

typedef struct NodeImpl Node;

typedef struct {
    Node dummy;
} List;

void ListInit(List* list)
{
    assert(list != NULL);
    list->dummy.next = NULL;
}

Node* ListDummy(List* list)
{
    assert(list != NULL);
    return &list->dummy;
}

Node* ListHead(List* list)
{
    assert(list != NULL);
    return list->dummy.next;
}

Node* ListNext(Node* node)
{
    return (node == NULL)? NULL : node->next;
}

void ListFree(List* list)
{
    assert(list != NULL);
    Node* node = list_head(list);
    while (node != NULL)
    {
        Node* next = list_next(node);
        free(node);
        node = next;
    }
}

Data_t ListHet(Node* node)
{
    assert(node != NULL);
    return node->data;
}

Data_t* ListGetPtr(Node* node)
{
    assert(node != NULL);
    return &node->data;
}
.
Node* InsertAfter(Node* pivot, Data_t to_insert)
{
    assert(pivot != NULL);
    Node* new = malloc(sizeof(Node)); //Needs to check
    new->next = pivot->next;
    new->data = to_insert;

    pivot->next = new;

    return new;
}

bool RemoveNext(Node* pivot, Data_t* result)
{
    assert(pivot != NULL);

    Node* next = pivot->next;
    if (next == NULL)
    {
        return false;
    }

    pivot->next = next->next;

    if (result != NULL)
    {
        *result = next->data;
    }

    free(next);

    return true;
}

//----------------------------------------------------
// End of list
//----------------------------------------------------



//--------------------------------------------------
// Binary Search Tree
//--------------------------------------------------

typedef int key_t;
typedef int value_t;
typedef uint32_t node_t;

#define NULL_NODE ((node_t) 0xFFFFFFFFU)

typedef struct {
    node_t parent_i;
    node_t lchild_i;
    node_t rchild_i;
    key_t key;
    value_t value;
} TreeNode;

typedef struct {
    // Dynamic array of nodes:
    TreeNode* nodes;
    size_t size;
    size_t capacity;

    // Tree root:
    node_t root_i;
} Tree;

void InitTree(Tree* tree)
{
    assert(tree);

    tree->size     = 0U;
    tree->capacity = 1U;

    tree->nodes = calloc(tree->capacity, sizeof(TreeNode)); //todo: check

    tree->root_i = NULL_NODE;
}

void FreeTree(Tree* tree)
{
    assert(tree);

    free(tree->nodes);
    tree->nodes = NULL;
}

TreeNode* tree_get(Tree* tree, node_t node_id)
{
    assert(tree);
    assert(node_i != NULL_NODE);

    return &tree->nodes[node_id];
}

void TransplantTree(Tree* tree, node_t transplanted_i, node_t new_i)
{
    TreeNode* transplanted = tree_get(tree, transplanted_i);

    if (transplanted->parent_i == NULL_NODE)
    {
        tree->root_i = new_i;
    }
    else
    {
        TreeNode* parent = tree_get(tree, transplanted->parent_i);
        if (transplanted_i == parent->lchild_i)
        {
            parent->lchild_i = new_i;
        }
        else
        {
            parent->rchild_i = new_i;
        }
    }

    if (new_i != NULL_NODE)
    {
        TreeNode* new = tree_get(tree, new_i);
        new->parent_i = transplanted->parent_i;
    }
}

node_t AllocateTreeNode(Tree* tree)
{
    assert(tree);
:
    if (tree->size == tree->capacity)
    {
        size_t new_capacity =
            (tree->size == 0U)? 1U : (2U * tree->capacity);

        tree->nodes = realloc(tree->nodes, new_capacity * sizeof(TreeNode));
        tree->capacity = new_capacity;
    }

    node_t allocated_i = tree->size;
    tree->size += 1U;

    TreeNode* allocated = tree_get(tree, allocated_i);

    allocated->parent_i = NULL_NODE;
    allocated->lchild_i = NULL_NODE;
    allocated->rchild_i = NULL_NODE;

    return allocated_i;
}

void FreeTreeNode(Tree* tree, node_t freed_id)
{
    assert(tree);
    TreeNode* freed = TreeGet(tree, freed_id);

    freed->parent_i = NULL_NODE;
    freed->lchild_i = NULL_NODE;
    freed->rchild_i = NULL_NODE;

    node_t last_i = tree->size - 1U;
    if (freed_i < tree->size - 1U)
    {
        TreeNode* last = tree_get(tree, last_i);

        *freed = *last;

        tree_transplant(tree, last_i, freed_id);
    }

    tree->size -= 1U;

}

node_t SearchTreeNode(Tree* tree, key_t search_key, node_t* parent_i)
{
    assert(tree);
    assert(parent_i != NULL);
    *parent_i = NULL_NODE;


    node_t cur_i = tree->root_i;
    while (cur_i != NULL_NODE)
    {
        TreeNode* node = tree_get(tree, cur_i);

        if (search_key == node->key)
        {
            return cur_i;
        }

        *parent_i = cur_i;
        if (search_key < node->key)
        {
            cur_i = node->lchild_i;
        }
        else
        {
            cur_i = node->rchild_i;
        }
    }

    return NULL_NODE;
}

node_t tree_minimum(Tree* tree, node_t subtree_i)
{
    assert(subtree_i != NULL_NODE);

    TreeNode* subtree = tree_get(tree, subtree_i);
    while (subtree->lchild_i != NULL_NODE)
    {
        subtree_i = subtree->lchild_i;
        subtree = tree_get(tree, subtree_i);
    }

    return subtree_i;
}

bool SearchTree(Tree* tree, key_t key, value_t* res)
{
    assert(tree);
    assert(res);

    node_t parent_i = NULL_NODE;
    node_t found_i = tree_search_node(tree, key, &parent_i);
    if (found_i == NULL_NODE)
    {
        return false;
    }

    TreeNode* found = tree_get(tree, found_i);
    *res = found->value;

    return true;
}


void Set(Tree* tree, key_t key, value_t value)
{
    // Search by the key:
    node_t parent_i = NULL_NODE;
    node_t found_i = tree_search_node(tree, key, &parent_i);

    if (found_i != NULL_NODE)
    {
        TreeNode* found = tree_get(tree, found_i);
        found->value = value;

        return;
    }

    node_t allocated_i = tree_node_allocate(tree);

    if (tree->root_i == NULL_NODE)
    {
        tree->root_i = allocated_i;
    }
    else
    {
        TreeNode* parent = tree_get(tree, parent_i);
        if (key < parent->key)
        {
            parent->lchild_i = allocated_i;
        }
        else if (parent->key < key)
        {
            parent->rchild_i = allocated_i;
        }
    }

    TreeNode* allocated = tree_get(tree, allocated_i);

    allocated->parent_i = parent_i;
    allocated->key      = key;
    allocated->value    = value;
}

bool Remove(Tree* tree, key_t key, value_t* ret)
{
    node_t selected_parent_i = NULL_NODE;
    node_t selected_i = tree_search_node(tree, key, &selected_parent_i);

    if (selected_i == NULL_NODE)
    {
        return false;
    }

    TreeNode* selected = GetTree(tree, selected_i);

    if (selected->lchild_i == NULL_NODE)
    {
        TransplantTree(tree, selected_i, selected->rchild_i);
    }
    else if (selected->rchild_i == NULL_NODE)
    {
        TransplantTree(tree, selected_i, selected->rchild_i);
    }
    else
    {
        node_t minimum_i  = tree_minimum(tree, selected->rchild_i);
        TreeNode* minimum = tree_get(tree, minimum_i);

        if (minimum->parent_i != selected_i)
        {
            TransplantTree(tree, minimum_i, minimum->rchild_i);

            minimum->rchild_i = selected->rchild_i;
            TreeNode* right = GetTree(tree, minimum->rchild_i);
            right->parent_i = minimum_i;
        }

        TransplantTree(tree, selected_i, minimum_i);

        minimum->lchild_i = selected->lchild_i;
        TreeNode* left = tree_get(tree, minimum->lchild_i);
        left->parent_i = minimum_i;
    }

    *ret = selected->value;

    tree_node_free(tree, selected_i);

    return true;
}

//--------------------------------------------------
// End of Binary Search Tree
//--------------------------------------------------
