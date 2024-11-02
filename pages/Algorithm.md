- Data Structure: https://www.cnblogs.com/Ck-0ff/p/15712553.html
- Data Structure
  collapsed:: true
	- Data
	  collapsed:: true
		- Logical and Physical Structure of Data
		  collapsed:: true
			- logical structure: set structure, linear structure, tree structure, and graph structure
			- physical structure(storage structure): Sequential storage structure, Chain storage structure
		- Algorithm characteristics and Time Complexity
		  collapsed:: true
			- five characteristics
			  collapsed:: true
				- **Input** : The algorithm has 0 or more inputs
				- **Output** : The algorithm has at least 1 or more outputs
				- **Finiteness** : The algorithm will automatically end after a limited number of steps without looping infinitely, and each step can be completed within an acceptable time.
				- **Deterministic** : Each step in the algorithm has a definite meaning and there is no ambiguity.
				- **Feasibility** : Each step of the algorithm is feasible, which means that each step can be executed a limited number of times.
			- time complexity
			  collapsed:: true
				- Time frequency: The number of statement executions in an algorithm is called statement frequency or time frequency. Denote it as **T(n)** .
				- If there is a certain auxiliary function f(n), when n approaches infinity, T ( The limit value of n)/f(n) is a constant that is not equal to zero, then f(n) is said to be a function of the same order of magnitude as T(n), recorded as T(n)=O(f(n)), which is called The asymptotic time complexity of the algorithm, referred to as **time complexity** .
			- common order of magnitude
			  collapsed:: true
				- ![](https://img-blog.csdnimg.cn/b0a9352ec69a4eff8d530e088b6e54c9.png)
			- compute time complexity
			  collapsed:: true
				- Basic operations, that is, only constant terms, are considered to have a time complexity of O(1)
				- Sequential structure, time complexity is calculated by addition.
				- Loop structure, time complexity is calculated by multiplication
				- Branch structure, when taking the maximum value of time complexity to judge the efficiency of an algorithm, you often only need to focus on the highest order term of the number of operations, and other minor terms and constant terms can be ignored
				- Unless otherwise specified, the time complexity of the algorithms we analyze refers to the worst time complexity.
			- Space complexity
	- Linear Lists
	  collapsed:: true
		- characteristics:
		  collapsed:: true
			- limit in number, in a logical sequence, data elements, single
			- same data type, occupy same amount of storage space
			- abstract
		- ***A linear list is a logical structure that represents a one-to-one adjacent relationship between elements. Sequence lists and linked lists refer to storage structures. They are concepts at different levels, so do not confuse them.***
		- operations
		  collapsed:: true
			- InitList(&L): Initialization list. Construct an empty linear list
			- Length(L): Find the length of the list. Returns the length of the linear list, that is, the number of data elements in L.
			- LocateElem(L,e): Find operation by value. Find elements with a given key value in list L.
			- GetElem(L,i): bitwise search operation. Get the value of the element at position 1 in the list.
			- ListInsert(&L,i,e): Insertion operation. Insert the specified element e at the i-th position in list L.
			- ListDelete(&L,i,&e): Delete operation. Delete the element at position i in list L and use e to return the value of the deleted element
			- PrintList(I): Output operation. Output all element values ​​of linear list L in front and back order
			- Empty(L): Empty operation. If L is an empty table, return true, otherwise return false.
			- DestroyList(&L): Destroy operation. Destroy the linear list and release the memory space occupied by linear list L.
		- Two Storage structure
		  collapsed:: true
			- ![](https://img-blog.csdnimg.cn/cf35e5b386da414db5cf214e90e37052.png){:height 191, :width 349}
		- Sequence table
		  collapsed:: true
			- code
			  collapsed:: true
				- ```#define LIST_INIT_SIZE 100  //顺序表存储空间的初始分配量
				  #define LISTINCREMENT 10    //顺序表存储空间的分配增量
				  typedef struct {
				       ElemType *elem;   //存储空间的基地址
				       int length;       //顺序表的当前长度
				       int listsize;     //数组存储空间的长度
				  }SqList;
				  //初始化
				  Status InitList(SqList &L){
				      L.elem=(ElemType *)malloc(LIST_INIT_SIZE*sizeof(ElemType));
				      if(!L.elem) exit(OVERFLOW);
				      L.length=0;
				      L.listsize=LIST_INIT_SIZE;
				      return OK;
				  }
				  //销毁
				  void DestroyList(SqList &L) {
				    if(L.elem) free(L.elem);
				    L.elem=NULL;
				  }
				  //清零
				  void ClearList(SqList &L)
				  {
				         L.length=0;
				  }
				  //判空
				  Status ListEmpty(SqList L)
				  {
				      if(L.length==0) return TRUE;
				      else return FALSE;
				  }
				  //求表长
				  int ListLength(SqList L)
				  {
				       return L.length;
				  }
				  //取值
				  Status GetElem(SqList L,int i,ElemType &e){
				      if(i<1 || i>L.length) return ERROR;
				      e=L.elem[i-1];       
				      return OK;
				  }
				  //定位1
				  int LocateElem (SqList L,ElemType e)
				  {
				      i=1;
				      while( i<=L.length && L.elem[i-1]!=e)  i++;
				      if(i<=L.length) return i;  
				      else return 0;       
				  } 
				  	//定位2
				  	/*int LocateElem (SqList L, ElemType e, Status (*compare)(ElemType, ElemType))
				  	{
				  		i=1;  p=L.elem;
				  		while(i<=L.length && !(*compare)(*p++,e)) ++i;
				  		if(i<=L.length) return i;
				  		else return 0;
				  	} 
				  	//定义比较函数：
				  	Status GT(ElemType a,ElemType b){
				  		 if(a>b) return TRUE;
				  		 else return FALSE;
				  	}
				  	*/
				  //插入
				  Status ListInsert(SqList &L,int i,ElemType e){
				      //插入元素e到顺序表L的第i个位置之前
				      if(i<1 ||i>L.length+1) return ERROR;
				      if(L.length==L.listize)
				       {
				  	 L.elem=(ElemType *) realloc(L.elem, (L.listsize+LISTINCREMENT)*sizeof(ElemType) );
				  	if(!L.elem) exit(OVERFLOW);
				  	L.Listsize += LISTINCREMENT;
				  	 }//重新分配空间
				      q=&L.elem[i-1];
				      for(p=&(L.elem[L.length-1];p>=q;--p) 
				           *(p+1)=*p;
				      *q=e;
				      ++L.length;
				      return OK;
				  }
				  Status ListDelete(SqList &L,int i,ElemType &e){
				      //删除顺序表L中的第i个元素，其值由e返回;
				      if(i<1 ||i>L.length) return ERROR;
				      p=&L.elem[i-1];
				      e=*p;
				      q=L.elem+L.length-1;
				      for(++p;p<=q;++p) 
				         *(p-1)=*p;
				      --L.length;
				      return OK;
				  }
				  Status ListTraverse(SqList L,Status (*visit)(ElemType))
				  {
				    for(i=0;i<L.length;i++)
				      if(!visit(L.elem[i])) return ERROR;
				    return OK;
				  } 
				  //定义访问函数：
				  Status visit(ElemType e){
				       printf(e);
				       return OK;
				  }
				  
				  ```
			- Characteristics: In terms of logical relationships, two adjacent elements are also adjacent in physical location.
			- Advantages:
			  collapsed:: true
				- Random access to any element in the table is possible.
				- Most operations are relatively easy to implement.
				- High storage utilization (the relationship is implicit in the storage structure, no need for explicit representation).
			- Disadvantages:
			  collapsed:: true
				- When the scale of the table is unknown, it is difficult to determine the size of two constants.
				- Sequential tables require the movement of a large number of elements during insertion and deletion operations, which wastes a lot of time.
			- Appropriate occasions:
			  collapsed:: true
				- The table's elements do not change frequently, but fast access to elements is needed.
				- Or when the storage space requirement is known in advance.
		- Linked List
		  collapsed:: true
			- characteristics
			  collapsed:: true
				- There is no fixed connection between the storage locations of any two elements. The storage location of each element can only be pointed out by the pointer of its immediate predecessor node.
				- To obtain the i-th element in a singly linked list, you must start from the head pointer and search backward along the pointer chain. Therefore, a singly linked list is a sequential access storage structure.
			- code
			  collapsed:: true
				- id:: 670f203e-0825-425c-93f2-678036b3c63a
				  ```
				  typedef struct LNode{
				     ElemType data;       //数据域
				     struct LNode *next;  //指针域
				  }LNode,*LinkList;
				  
				  Status InitList(LinkList &L) {
				      L=(LinkList)malloc(sizeof(LNode));
				      if (!L) exit(OVERFLOW);
				      L->next=NULL;
				      return OK;
				  }
				  
				  void DestroyList(LinkList &L) {
				      while(L) {
				  	p=L->next;
				  	free(L);
				  	L=p;
				      }
				  }
				  
				  void ClearList(LinkList L) {
				    for(p=L->next;p; p=L->next) {
				  	L->next=p->next;
				  	free(p);
				    }
				  }
				  Status ListEmpty(LinkList L) {
				      if( !L->next) return TRUE;
				      else return FALSE;
				  }
				  int ListLength(LinkList L) {
				      p=L->next; n=0;   
				      while(p)
				      { n++;	p=p->next; }
				      return n;
				  }
				  Status GetElem(LinkList L, int i, ElemType &e) {
				      j=1;
				      p=L->next; 
				      while(p &&j<i) { p=p->next; ++j;}
				      if( !p || j>i) 
				        return ERROR;   
				      e=p->data ;
				      return OK;
				   } 
				  LinkList LocateElem(LinkList L,ElemType e,Status (*compare)(ElemType,ElemType)) {
				      p=L->next;
				      while( p && !compare(p->data,e) ) p=p->next;
				      return p;
				  }
				  //定义比较函数：
				  Status LT(ElemType a,ElemType b) {
				       if(a<b) return TRUE;
				       else return FALSE;
				  }
				  //遍历
				  Status ListTraverse(LinkList L，Status (*visit)(ElemType)) {
				      for( p=L->next; p; p=p->next )
				        if( !visit(p->data) ) return ERROR;
				      return OK;
				  }
				  ```
			- Advantages:
			  collapsed:: true
				- In linked lists, insertion and deletion operations no longer require moving a large number of elements; only the pointers need to be modified.
				- There is no longer a need to define two constants.
			- Disadvantages:
			  collapsed:: true
				- Elements can only be accessed sequentially.
				- Most operations have a time complexity of O(n); this is because each operation often requires traversing the list from the beginning to the desired element.
				- Lower storage utilization (when storing elements, additional space is required to store the pointers).
			- Appropriate occasions:
			  collapsed:: true
				- When elements in the table are frequently inserted and deleted.
				- Or for applications where storage space requirements are not fixed.
			- types
			  collapsed:: true
				- **singly linked list**
				- **doubly linked list**
				- **Single circular linked list**: traversed from any node, without a head pointer but with a tail pointer
				  collapsed:: true
					- ![](https://img-blog.csdnimg.cn/6a88c03b916242e68530d89e239cac53.png)
				- **double circular linked list**
				  collapsed:: true
					- ![](https://img-blog.csdnimg.cn/6f39a3f080b2410ba7a6e6cf7dddb300.png){:height 130, :width 605}
		- Linear List Analysis
		  collapsed:: true
			- Access (Read/Write) Methods
			  collapsed:: true
				- sequential tables can access elements both sequentially and randomly
				- linked lists can only access elements sequentially starting from the head
			- Logical and Physical Structures
			  collapsed:: true
				- When using sequential storage, elements that are logically adjacent have corresponding physically adjacent storage locations.
				- when using linked storage, elements that are logically adjacent do not necessarily have physically adjacent storage locations
			- Search, Insertion, and Deletion Operations
			  collapsed:: true
				- search
				  collapsed:: true
					- value-based search:
						- unordered both O(n)
						- ordered: sequential table can use binary search O(logn)
					- index-based search:
						- sequential tables support random access, with a time complexity of O(1)
						- the average time complexity for linked lists is O(n)
				- insertion and deletion
				  collapsed:: true
					- The insertion and deletion operations in a sequential table require moving half the length of the table's elements on average.
					- For linked lists, insertion and deletion operations only require modifying the pointer fields of the relevant nodes.
			- Space Allocation
			  collapsed:: true
				- sequential storage: once the storage space is full, it cannot be expanded. If new elements are added, memory overflow -> pre-allocate a sufficiently large storage space.
				  collapsed:: true
					- If the pre-allocated space is too large, it may lead to a large amount of idle space
					- if it is too small, it will cause overflow
					- Dynamic storage allocation can expand the storage space, but it requires moving a large number of elements, which reduces operational efficiency
					- if there is no larger block of continuous storage space in memory, allocation failure
				- In linked storage, node space is allocated only when needed, and as long as there is space in memory, it can be allocated. This approach is flexible and efficient.
	- Stacks & Queues
	  id:: 670f0f3e-ec8e-4cc5-bd9e-e7e759e1ef82
	  collapsed:: true
		- overview
		  collapsed:: true
			- ![](https://img-blog.csdnimg.cn/3153fb7e59be42a2b448cc8757377233.png){:height 221, :width 385}
		- stack
		  collapsed:: true
			- only two actions: push and pop
			  collapsed:: true
				- ![](https://img-blog.csdnimg.cn/6344e612c71b4ee8be592d8c18ec9cbc.png){:height 166, :width 175}
			- **The stack is a last-in-first-out (LIFO) linear list.**
			- operations
			  collapsed:: true
				- InitStack(&S)
				- StackEmpty(S)
				- StackLength(&S)
				- GetTop(S, &e)
				- Push(&S,e)
				- Pop(&S, &e)
				- ClearStack(&S)
				- StackTraverse(S, visit())
				- DestroyStack(&S)
			- code
			  collapsed:: true
				- ```Status InitStack(SqStack &S) {
				  //构造一个空栈，该栈由指针S指示 
				    S.base=(SElemType*)malloc ((STACK_INIT_SIZE)*sizeof(SElemType));
				      // 栈的连续空间分配
				      //S.base=new SElemType[STACK_INIT_SIZE]; 等价
				  	if(!S.base)	exit(OVERFLOW);
				  	S.top=S.base; //空栈，初始化栈顶指针
				  	S.stacksize=STACK_INIT_SIZE; 
				  	return OK;
				  }//InitStack 
				  ```
		- queue
		  collapsed:: true
			- **A queue is a first-in-first-out (FIFO) linear list.**
			- concepts
			  collapsed:: true
				- queue: A linear table that only allows insertions at one end of the table and deletions at the other end.
				- Head of the team: the end that is allowed to be deleted
				- Tail of the queue: the end that allows insertion
				- Empty queue: a queue with no elements
				- ![](https://img-blog.csdnimg.cn/bdef8e9851824a62bb02ae8d434cee74.png){:height 112, :width 409}
			- operations
			  collapsed:: true
				- ![](https://img-blog.csdnimg.cn/dc7e017ae40d475b88f9ed1979d15218.png)
	- LATER Strings
		- basic concepts
		  collapsed:: true
			- ![](https://img-blog.csdnimg.cn/6b12538323134de482e13ea327698530.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ0tfMGZm,size_1,color_FFFFFF,t_70,g_se,x_16){:height 201, :width 389}{:height 201, :width 389}
		- operations
		  collapsed:: true
			- ![](https://img-blog.csdnimg.cn/edc62f855f7f46939692bc45cce061ae.png){:height 241, :width 387}
	- Arrays
		- Calculation of array subscripts
		  collapsed:: true
			- Column-major order (columns first):
			  collapsed:: true
				- Elements are stored in the order of increasing row numbers, for each column sequentially.
				- LOC(i,j) = LOC(0,0) + (i*m + j) * L;
			- Row-major order (rows first):
			  collapsed:: true
				- Elements are stored in the order of increasing column numbers, for each row sequentially.
				- LOC(i,j) = LOC(0,0) + (i*n + j) * L;
		- Special matrix compression storage
		  collapsed:: true
			- symmetric matrix / sparse matrix, upper (lower) triangular matrix
			  collapsed:: true
				- only the data on one side of the diagonal (including the diagonal) needs to be stored in the array
				- store the elements in the lower triangle
					- ![](https://img-blog.csdnimg.cn/e0539897a7b44ad9aae13ba6a1d5c27c.png){:height 74, :width 232}
				- store the elements of the upper triangle
					- ![](https://img-blog.csdnimg.cn/9c80a82595234788b41668feaefc65cb.png){:height 73, :width 227}
			- triple sequence table
			  collapsed:: true
				- ```
				  typedef  struct
				  {
				      int i,j;     //行坐标、列坐标
				      ElemType e;  //元素
				  }Triple;
				  typedef struct
				  {
				      Triple date[MAXSIZE+1];  //0不存储元素
				      int mu,nu,tu;      //行数、列数、非零元个数
				  }TSMatrix;
				  ```
				- ```
				     0 12  9  0  0  0  0
				     0  0  0  0  0  0  0
				    -3  0  0  0  0 14  0
				  M= 0  0 24  0  0  0  0
				     0 18  0  0  0  0  0
				    15  0  0 -7  0  0  0
				  
				  //上面矩阵用三元组表示
				  i  j  v      
				  1  2  12
				  1  3  9
				  3  1  -3
				  3  6  14
				  4  3  24 
				  5  2  18
				  6  1  15
				  6  4  -7
				  ```
			- Transpose: Transpose in column-major order
			  collapsed:: true
				- code
				  collapsed:: true
					- determine its position in the transposed array by traversing the subscripts of all elements
					- ```
					  void TransposeSMatrix(TSMatrix *T1,TSMatrix *T2)
					  {
					      T2->mu=T1->nu;T2->nu=T1->mu;T2->tu=T1->tu;
					      if(T1->tu)
					      {
					          int q=1,col,p;
					          for(col=1;col<=T1->nu;col++)  //矩阵列循环
					          {
					              for(p=1;p<=T1->tu;p++)    //遍历所有元素
					              {
					                  if(T1->date[p].j==col)  //当元素在col列时
					                  {
					                      T2->date[q].i=T1->date[p].j;
					                      T2->date[q].j=T1->date[p].i;
					                      T2->date[q].e=T1->date[p].e;
					                      q++;
					                  }
					              }
					          }
					      }
					  }
					  //上述代码，当矩阵运算为满时，即tu=mu*nu,其时间复杂度为O(nu*nu*mu)
					  ```
				- Quick transpose
				  collapsed:: true
					- predetermine the first non-zero element of each column in the corresponding transposed array date position in
					- two auxiliary arrays are required
					- num[ ]: used to store the number of non-zero elements in each column
					- cpot[ ]: stores the position of the first non-zero element in the transposed array date
					- code
					  collapsed:: true
						- ```
						  void FastTransposeSMatrix(TSMatrix *T1,TSMatrix *T2)
						  {
						      int num[T1->nu],cpot[T1->nu];
						      int col,p,q,t;
						      T2->mu=T1->nu;T2->nu=T1->mu;T2->tu=T1->tu;
						      if(T1->tu)
						      {
						          //初始化每列非零元个数为0
						          for(col=1;col<=T1->nu;col++)
						          {
						              num[col]=0;
						          }
						          //求每列非零元个数
						          for(t=1;t<=T1->tu;t++)
						          {
						              ++num[T1->date[t].j];
						          }
						          //求每列第一个非零元转置后的位置
						          cpot[1]=1;
						          for(col=2;col<=T1->nu;col++)
						          {
						              cpot[col]=num[col-1]+cpot[col-1];
						          }
						          //遍历所有元素
						          for(p=1;p<=T1->tu;p++)
						          {
						              col=T1->date[p].j;  //获取列坐标
						              q=cpot[col];        //获取新位置
						              T2->date[q].i=T1->date[p].j;
						              T2->date[q].j=T1->date[p].i;
						              T2->date[q].e=T1->date[p].e;
						              ++cpot[col];   //之所以这个地方要++，因为每列非零元可能不止一个
						          }  
						      }
						  }
						  ```
	- Trees and Binary Trees
		- **Properties of binary trees** :
		  collapsed:: true
			- There are at most 2^(i-1) nodes on the i-th level of the binary tree
			- A binary tree with depth K has at most (2^k)-1 nodes.
			- The number of nodes with degree 2 in any binary tree is 1 less than the number of nodes with degree 0.
			- The depth of a complete binary tree with n nodes is | log2n | +1
			- Then if there is a left child, the number is 2i, if there is a right child, the number is 2i+1, and the mother node is i/2.
		- Full Binary Tree:
		  collapsed:: true
			- A binary tree with a depth of k that contains (2^k)−1 nodes.
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410161118356.png){:height 138, :width 223}
		- Complete Binary Tree:
		  collapsed:: true
			- A binary tree with n nodes that corresponds one-to-one with the nodes numbered from 1 to n in a full binary tree.
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410161119941.png){:height 167, :width 230}
		- storage structure of trees
		  collapsed:: true
			- **Sequential storage structure** :
			  collapsed:: true
				- suitable for complete binary trees
				- For a general binary tree, it may cause a huge waste of storage space.
			- **chain storage**
			  collapsed:: true
				- ```
				    typedef struct BiTNode{
				      ElemType data;
				      struct BiTNode *lchild,*rchild;
				    }BiTNode,*BiTree;
				  ```
				- There are n+1 empty link fields in the binary linked list of a binary tree with n nodes.
		- Binary tree traversal
		  collapsed:: true
			- Traverse a binary tree: Patrol each node in the binary tree according to a certain search path, so that each node is visited once and only once.
			- **preorder traversal**: root->left subtree->right subtree
			  collapsed:: true
				- ```
				  void PreorderTraverse(BiTree T, void (*visit)(ElemType)) {
				  	if(T){
				           visit(T->data); 
				           PreorderTraverse(T->lchild,visit);                  
				           PreorderTraverse(T->rchild,visit);                         
				      }
				  }//PreorderTraverse
				  ```
				- ```
				  Status CreateBiTree(BiTree &T)
				  { //先序创建一棵二叉树，由指针T指向其根结点的指针
				      scanf("%c",&ch);
				      if(ch=='#')  T=NULL;
				      else {
				  	   if(!(T=(BiTree)malloc(sizeof(BiTNode))))  exit(OVERFLOW);
				  	   T->data=ch;
				  	   CreateBiTree(T->lchild);
				  	   CreateBiTree(T->rchild);
				      }
				      return OK;
				  }//CreateBiTree
				  ```
			- **inorder traversal**: left subtree->root->right subtree
			  collapsed:: true
				- non-recursive
				- ```
				  void InorderTraverse(BiTree T, void (*visit)(ElemType))
				  {     InitStack(S); Push(S,T); 
				         while(!StackEmpty(S)) 
				         {  while(GetTop(S,p)&&p)    Push(S,p->lchild); 
				            Pop(S,p); 
				            if(!StackEmpty(S))
				  　　      {   
				  　　       Pop(S,p); 
				  	      visit(p->data);
				  	      Push(S,p->rchild); 
				               }
				          }
				  } //InOrderTraverse
				  ```
			- **postorder traversal**:left subtree->right subtree->root
			  collapsed:: true
				- non-recursive
				- ```
				  void PostorderTraverse(BiTree T, void (*visit)(ElemType)) {
				        InitStack(S); Push(S,T); 
				        while(!StackEmpty(S)) {
				            while(GetTop(S,p)&&p)    Push(S,p->lchild); 
				            Pop(S,p); 
				            while((i=GetTop(S,r)) && r->rchild==p) {
				  　　       visit(r->data);
				                Pop(S,p); 
				            }//while
				            if(i) Push(S,r->rchild); 
				         }//while
				  } //PostOrderTraverse
				  ```
			- Level-order traversal of a binary tree
			  collapsed:: true
				- ```
				  void LevelorderTraverse(BiTree T, void (*visit)(ElemType))
				  {//层序遍历二叉树
				      p=T;
				      InitQueue(Q);
				      if(p) EnQueue(Q,p);
				      while(!QueueEmpty(Q))  // 队列不空
				     {
				  	DeQueue(Q,p);  
				  	 visit(p->data);
				  	if(p->lchild)  
				  	 EnQueue(Q,p->lchild);      
				  	if(p->rchild)  
				  	 EnQueue(Q,p->rchild);      
				      }
				  }// LevelorderTraverse 
				  ```
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410161128718.png){:height 274, :width 167}
			- Time Complexity Analysis: For a binary tree with n nodes, the time complexity of various traversal algorithms is O(n).
			- Applications: It forms the basis for various operations on binary trees, such as finding the parent of a node, finding the children of a node, determining the level of a node, and counting the number of nodes.
			- Essence: It is the linearization of a non-linear structure.
		- Binary trees and expressions
		  collapsed:: true
			- How to express an expression using a binary tree
			- ```
			  例：a+b*(c-d)-e/f
			  描述表达式的二叉树遍历序列
			  前缀表达式（波兰式）：
			          -+a*b-cd/ef
			  中缀表达式：
			          a+b*c-d-e/f
			  后缀表达式（逆波兰式）：
			          abcd-*+ef/-
			  ```
			- ![](https://img-blog.csdnimg.cn/24737fb431a945e5bac7965575d6288c.png){:height 182, :width 147}
			- ?
		- Interchange of trees, binary trees, and forests
		  collapsed:: true
			- tree to binary tree
			  collapsed:: true
				- Add a line between sibling nodes of the tree
				- Only the connection between the parent node and the first child node is retained in the tree.
				- ![](https://img-blog.csdnimg.cn/e17224b184e0440c974cf43f8b981287.png){:height 153, :width 256}
			- Forest to binary tree:
			  collapsed:: true
				- Convert all trees in the forest into binary trees
				- Starting from the second tree, insert the converted binary tree into the first tree as the right subtree of the root node of the first tree.
			- Convert binary trees to trees and forests
			  collapsed:: true
				- Add lines. Add a line between all sibling nodes.
				- Delete lines. Each node in the tree only retains the connection between it and the first child node, and deletes the connections between other child nodes.
				- Adjust. With the root node of the tree as the axis, adjust the entire tree (the first child is the left child of the node, and the child whose brother turns over is the right child of the node)
		- Huffman tree / optimal tree / optimal binary tree
		  collapsed:: true
			- WPL:
			  collapsed:: true
				- The weighted path length of a tree is the sum of the weighted path lengths of all leaf nodes in the tree. Usually noted as "WPL".
				- ![](https://img-blog.csdnimg.cn/05ac6a202501428bb95aecbfc47bb7a4.png){:height 67, :width 125}
			- process
			  collapsed:: true
				- Step1: According to the given n weights {w1, w2, …, wn}, construct a set of n binary trees F = {T1, T2, …, Tn}, Ti (1≤i≤n) has only one weighted value wi The root node of , its left and right subtrees are both empty.
				- Step2: Select two binary trees with the smallest root node weights in F and construct a new binary tree as the left and right subtrees respectively. Set the weight of the root node of the new binary tree to the sum of the weights of the root nodes on the left and right subtrees.
				- Step3: Delete these two binary trees in F and add the new binary tree to F.
				- Step4: Repeat step 2 and step 3. Until there is only one tree left in F. This tree is the Huffman tree.
			- code
			  collapsed:: true
				- ```
				  typedef struct
				  {
				  unsigned int weight; 
				  unsigned int parent, lchild, rchild; 
				  }HTNode,*HuffmanTree;
				  
				  Status CreateHuffmanTree(HuffmanTree &HT, int *w, int n) 
				  {  if(n<=1) return ERROR; 
				     m=2*n-1; 
				     HT=(HuffmanTree)malloc((m+1)*sizeof(HTNode));//0号单元未用
				     for(p=HT+1,i=1;i<=n; ++i,++p,++w)    *p={*w,0,0,0}; 
				     for(;i<=m; ++i, ++p)     *p= {0,0,0,0}; 
				     for(i=n+1;i<=m; ++i)
				     { Select(HT,i-1,s1,s2);
				       HT[s1].parent=i; HT[s2].parent=i;       
				       HT[i].lchild=s1; HT[i].rchild=s2;   
				       HT[i].weight=HT[s1].weight+HT[s2].weight; 
				     }	
				     return OK;
				  }
				  ```
			- characteristics
			  collapsed:: true
				- There are no nodes with degree 1 in the Huffman tree. This type of tree is also called a strict binary tree or a regular binary tree. There are only nodes with degree 0 and degree 2 in this type of binary tree.
		- Huffman coding
		  collapsed:: true
			- Huffman coding first needs to construct a binary tree based on the input text. The left link of the tree represents the bit "0", the right link represents the bit "1", and the leaf nodes represent characters. The Huffman encoding value corresponding to the character is the link value from the root node to the leaf node.
			  collapsed:: true
				- ![](https://img-blog.csdnimg.cn/ce08c94b2e8f40dbb888d851a326fa7e.png){:height 215, :width 354}
			- Build a Huffman tree
			  collapsed:: true
				- ```
				  #define LEN 512
				  struct huffman_node{
				          char c;
				          int weight;
				          char huffman_code[LEN];
				          huffman_node * left;
				          huffman_node * right;
				  };
				  ```
				- ```
				  int huffman_tree_create(huffman_node *&root, map<char, int> &word){
				          char line[MAX_LINE];
				          vector<huffman_node *> huffman_tree_node;
				  
				          map<char, int>::iterator it_t;
				          for (it_t = word.begin(); it_t != word.end(); it_t++){
				                  // 为每一个节点申请空间
				                  huffman_node *node = (huffman_node *)malloc(sizeof(huffman_node));
				                  node->c = it_t->first;
				                  node->weight = it_t->second;
				                  node->left = NULL;
				                  node->right = NULL;
				                  huffman_tree_node.push_back(node);
				          }
				  
				          // 开始从叶节点开始构建Huffman树
				          while (huffman_tree_node.size() > 0){
				                  // 按照weight升序排序
				                  sort(huffman_tree_node.begin(), huffman_tree_node.end(), sort_by_weight);
				                  // 取出前两个节点
				                  if (huffman_tree_node.size() == 1){// 只有一个根结点
				                          root = huffman_tree_node[0];
				                          huffman_tree_node.erase(huffman_tree_node.begin());
				                  }else{
				                          // 取出前两个
				                          huffman_node *node_1 = huffman_tree_node[0];
				                          huffman_node *node_2 = huffman_tree_node[1];
				                          // 删除
				                          huffman_tree_node.erase(huffman_tree_node.begin());
				                          huffman_tree_node.erase(huffman_tree_node.begin());
				                          // 生成新的节点
				                          huffman_node *node = (huffman_node *)malloc(sizeof(huffman_node));
				                          node->weight = node_1->weight + node_2->weight;
				                          (node_1->weight < node_2->weight)?(node->left=node_1,node->right=node_2):(node->left=node_2,node->right=node_1);
				                          huffman_tree_node.push_back(node);
				                  }
				          }
				          return 0;
				  }
				  ```
			- Character frequency statistics
			  collapsed:: true
				- ```
				  int read_file(FILE *fn, map<char, int> &word){
				          if (fn == NULL) return 1;
				          char line[MAX_LINE];
				          while (fgets(line, 1024, fn)){
				                  fprintf(stderr, "%s\n", line);
				                  //解析，统计词频
				                  char *p = line;
				                  while (*p != '\0' && *p != '\n'){
				                          map<char, int>::iterator it = word.find(*p);
				                          if (it == word.end()){// 不存在，插入
				                                  word.insert(make_pair(*p, 1));
				                          }else{
				                                  it->second ++;
				                          }
				                          p ++;
				                  }
				          }
				          return 0;
				  }
				  ```
			- Huffman tree to Huffman code
			  collapsed:: true
				- implement steps
					- read input;
					- Count the frequency of each character in the input;
					- According to the frequency, construct a Huffman tree;
					- Construct a compilation table for mapping characters to variable-length prefixes;
					- Encode the Huffman tree into a bit string and write it to the output stream;
					- Compress the data, i.e. use a compilation table to translate each text character, and write it to the output stream.
				- code
				  collapsed:: true
					- ```
					  int get_huffman_code(huffman_node *&node){
					          if (node == NULL) return 1;//层序遍历
					          huffman_node *p = node;
					          queue<huffman_node *> q;
					          q.push(p);
					          while(q.size() > 0){
					                  p = q.front();
					                  q.pop();
					                  if (p->left != NULL){
					                          q.push(p->left);
					                          strcpy((p->left)->huffman_code, p->huffman_code);
					                          char *ptr = (p->left)->huffman_code;
					                          while (*ptr != '\0'){
					                                  ptr ++;
					                          }
					                          *ptr = '0';
					                  }
					                  if (p->right != NULL){
					                          q.push(p->right);
					                          strcpy((p->right)->huffman_code, p->huffman_code);
					                          char *ptr = (p->right)->huffman_code;
					                          while (*ptr != '\0'){
					                                  ptr ++;
					                          }
					                          *ptr = '1';
					                  }
					          }
					          return 0;
					  }
					  ```
	- Graph
		- graph terminology G=(V,{E})
			- V: vertex finite non-empty set
			- VR: a collection of vertex relations
			- E is a finite set of edges (arcs)
			- n: number of vertices in the graph
			- e: number of edges or arcs
			- G: Figure
			- N: net
			- Undirected complete graph and directed complete graph:
			  collapsed:: true
				- undirected complete graph: in an undirected graph, if there is an edge between any two vertices, n(n-1)/2 edges
				- directed complete graph: in a directed graph, if there are two arcs with opposite directions between any two vertices, n(n-1) edges
			- Sparse graphs and dense graphs:
			  collapsed:: true
				- A graph with few edges or arcs is called a sparse graph, and the opposite is called a dense graph. The concept here is relative.
			- weight and net
			  collapsed:: true
				- weight: numbers associated with the edges, represent the distance or cost from one vertex to another
				- net: This weighted graph is often called a net.
			- Adjacent points:
			  collapsed:: true
				- G= (V, {E}), if the edge (v, v') belongs to E, then
				- the vertices v and v' are said to be adjacent points to each other=v and v' are adjacent=the edge (v, v') is attached to vertices v and v'=(v, v') is associated with vertices v and v'.
			- Degree, in-degree and out-degree:
			  collapsed:: true
				- The degree of point v is the number of edges associated with v, denoted as TD(v).
				- **the number of edges is actually half of the sum of the degrees of each vertex**
				- The number of arcs starting with vertex v is called the in-degree of v, denoted as ID (v);
				- the number of arcs starting with v as the tail is called the out-degree of v, denoted as OD (v);
				- the degree of vertex v is TD(v) =ID(v) +OD(v).
			- **Path and path length:**
				-
-