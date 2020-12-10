---
title: 图（邻接表）（BFS、DFS）
tags:
  - C
  - data structure
  - Algorithm
  - Graph
  - BFS
  - DFS

categories: 数据结构
comments: false
---

代码如下（附解析）：

<!--more-->

```c
#include<iostream>
#include<stdio.h>
#include<stdlib.h>
#include<iomanip>
using namespace std;
 
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define OVERFLOW -2
#define MAX_VERTEX_NUM 20//最大顶点个数
 
typedef int Status;
typedef int InfoType;
typedef char VertexType;
 
bool visited[MAX_VERTEX_NUM];
 
typedef struct ArcNode {
	int adjvex;//该弧所指向的顶点位置
	struct ArcNode *nextarc;//指向下一条弧的指针
	InfoType info;//该弧相关信息的指针
}ArcNode;
 
 
typedef struct VNode {
	VertexType data;//顶点信息
	ArcNode *firstarc;//指向第一条依附该顶点的弧的指针
}VNode,AdjList[MAX_VERTEX_NUM];
 
typedef struct {
	AdjList vertices;
	int vexnum, arcnum;//图的当前顶点数和弧数
	int kind;//图的种类标志
}ALGraph;
 
int LocateVex(ALGraph G, char v)
{
	int i;
	for (i = 0; i < G.vexnum; i++)
	{
		if (G.vertices[i].data == v)
			return i;
	}
	return -1;
}
/*
   采用邻接表存储表示，构造无向图G
*/
Status CreateUDG(ALGraph &G)
{
	int i, j, k, IncInfo;
	ArcNode *pi, *pj;
	char v1, v2;
	cout << "输入顶点数G.vexnum:";
	cin >> G.vexnum;
	cout << "输入边数G.arcnum:";
	cin >> G.arcnum;
	getchar();
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << "输入顶点G.vertices[" << i << "].data:";
		cin >> G.vertices[i].data;
		getchar();
		//初始化链表头指针为空
		G.vertices[i].firstarc = NULL;
	}
	/*
	   输入各边并构造邻接表
	*/
	for (k = 0; k < G.arcnum; ++k)
	{
		cout << "请输入第" << k + 1 << "条边的两个顶点:"<<endl;
		//输入一条边的起点和终点
		cin >> v1;
		cin >> v2;
		getchar();
		//确定V1和V2在G中的位置
		i = LocateVex(G, v1);
		j = LocateVex(G, v2);
		if (!(pi = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		pi->adjvex = j;
		pi->info = 0;
		//插入链表G.vertices[i]
		pi->nextarc = G.vertices[i].firstarc;
		G.vertices[i].firstarc = pi;
		
		if (!(pj = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		//插入链表G.vertices[j]
		pj->adjvex = i;
		pj->info = 0;
		pj->nextarc = G.vertices[j].firstarc;
		G.vertices[j].firstarc = pj;
	}//for
	return OK;
}//CreateUDG
 
//有向图
Status CreateDG(ALGraph &G)
{
	int i, j, k, IncInfo;
	ArcNode *pi, *pj;
	char v1, v2;
	cout << "输入顶点数G.vexnum:";
	cin >> G.vexnum;
	cout << "输入边数G.arcnum:";
	cin >> G.arcnum;
	getchar();
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << "输入顶点G.vertices[" << i << "].data:";
		cin >> G.vertices[i].data;
		getchar();
		//初始化链表头指针为空
		G.vertices[i].firstarc = NULL;
	}
	/*
	输入各边并构造邻接表
	*/
	for (k = 0; k < G.arcnum; ++k)
	{
		cout << "请输入第" << k + 1 << "条边的两个顶点:" << endl;
		//输入一条边的起点和终点
		cin >> v1;
		cin >> v2;
		getchar();
		//确定V1和V2在G中的位置
		i = LocateVex(G, v1);
		j = LocateVex(G, v2);
		if (!(pi = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		pi->adjvex = j;
		pi->info = 0;
		//插入链表G.vertices[i]
		pi->nextarc = G.vertices[i].firstarc;
		G.vertices[i].firstarc = pi;
	}//for
	return OK;
}//CreateDG
//有向网
Status CreateDN(ALGraph &G)
{
	int i, j, k, IncInfo;
	ArcNode *pi, *pj;
	char v1, v2;
	cout << "输入顶点数G.vexnum:";
	cin >> G.vexnum;
	cout << "输入边数G.arcnum:";
	cin >> G.arcnum;
	cout << "输入是否有Info信息（0、没有，1、有）";
	cin >> IncInfo;
	getchar();
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << "输入顶点G.vertices[" << i << "].data:";
		cin >> G.vertices[i].data;
		getchar();
		//初始化链表头指针为空
		G.vertices[i].firstarc = NULL;
	}
	/*
	输入各边并构造邻接表
	*/
	for (k = 0; k < G.arcnum; ++k)
	{
		cout << "请输入第" << k + 1 << "条边的两个顶点和相连边的权值:" << endl;
		//输入一条边的起点和终点
		cin >> v1;
		cin >> v2;
		cin >> IncInfo;
		getchar();
		//确定V1和V2在G中的位置
		i = LocateVex(G, v1);
		j = LocateVex(G, v2);
		if (!(pi = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		pi->adjvex = j;
		//插入链表G.vertices[i]
		pi->nextarc = G.vertices[i].firstarc;
		G.vertices[i].firstarc = pi;
		
		pi->info = IncInfo;
	}//for
	return OK;
}//CreateDN
//无向网
Status CreateUDN(ALGraph &G)
{
	int i, j, k, IncInfo;
	ArcNode *pi, *pj;
	char v1, v2;
	cout << "输入顶点数G.vexnum:";
	cin >> G.vexnum;
	cout << "输入边数G.arcnum:";
	cin >> G.arcnum;
	cout << "输入是否有Info信息（0、没有，1、有）";
	cin >> IncInfo;
	getchar();
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << "输入顶点G.vertices[" << i << "].data:";
		cin >> G.vertices[i].data;
		getchar();
		//初始化链表头指针为空
		G.vertices[i].firstarc = NULL;
	}
	/*
	输入各边并构造邻接表
	*/
	for (k = 0; k < G.arcnum; ++k)
	{
		cout << "请输入第" << k + 1 << "条边的两个顶点和相连边的权值:" << endl;
		//输入一条边的起点和终点
		cin >> v1;
		cin >> v2;
		cin >> IncInfo;
		getchar();
		//确定V1和V2在G中的位置
		i = LocateVex(G, v1);
		j = LocateVex(G, v2);
		if (!(pi = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		pi->adjvex = j;
		//插入链表G.vertices[i]
		pi->nextarc = G.vertices[i].firstarc;
		G.vertices[i].firstarc = pi;
		pi->info = IncInfo;
		if (!(pj = (ArcNode *)malloc(sizeof(ArcNode))))
			exit(OVERFLOW);
		//对弧结点赋邻接点"位置"信息
		pj->adjvex = i;
		//插入链表G.vertices[i]
		pj->nextarc = G.vertices[j].firstarc;
		G.vertices[j].firstarc = pi;
		pj->info = IncInfo;
	}//for
	return OK;
}//CreateUDN
 
 
Status CreateGraph(ALGraph &G)
{
	cout << "请输入图的种类：0表示DG，1表示DN，2表示UDG，3表示UDN" << endl;
	cin >> G.kind;
	switch (G.kind)
	{
	case 0:
		return CreateDG(G);
	case 1:
		return CreateDN(G);
	case 2:
		return CreateUDG(G);
	case 3:
		return CreateUDN(G);
	default:
		return ERROR;
	}
}
 
void list(ALGraph G)
{
	int i;
	ArcNode *p;
	cout << "输出邻接表（（0）代表无权值）：" << endl;
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << i << "：" << G.vertices[i].data;
		p = G.vertices[i].firstarc;
		while (p)
		{
			cout << setw(3) << p->adjvex;
			cout << "(" <<p->info<< ")";
			p = p->nextarc;
		}
		cout << endl;
	}
}
 
 
void DFS(ALGraph G, int v)
{
	ArcNode *p;
	int w;
	visited[v] = TRUE;
	cout << G.vertices[v].data << " ";
	
	for (p = G.vertices[v].firstarc; p; p = p->nextarc)
	{
		w = p->adjvex;
		if (!visited[w])//对v的尚未访问的邻接顶点w递归调用DFS
			DFS(G, w);
	}
}
//深度优先遍历
void DFSTraverse(ALGraph G)
{
	int v;
	for (v = 0; v < G.vexnum; ++v)
	{
		visited[v] = FALSE;
	}
	for (v = 0; v < G.vexnum; ++v)
	{
		if (!visited[v])
		{
			DFS(G, v);
			cout << endl;
		}
	}
}
 
void BFSTraverse(ALGraph G)
{
	int v,w;
	//辅助队列
	int front=0, rear = 0;
	VNode Queue[MAX_VERTEX_NUM];
	ArcNode *p;
 
	//初始化标志数组
	for (v = 0; v < G.vexnum; ++v)
	{
		visited[v] = FALSE;
	}
	for (v = 0; v < G.vexnum; ++v)
	{
		if (!visited[v])//v尚未访问
		{
			visited[v] =true;
			cout << G.vertices[v].data<<" ";
			Queue[++rear] = G.vertices[v];//v入队列
			while (front != rear)//判断队列是否为空
			{
				VNode u = Queue[++front];//队头元素出队列，并等于u
				cout << endl;
				/*
				判断该结点的后序表结点,存在即输出，入队列
				*/
				for (p = u.firstarc; p; p = p->nextarc)
				{
					w = p->adjvex;
					if (!visited[w])
					{
						visited[w] = true;
						cout << G.vertices[w].data << " ";
						Queue[++rear] = G.vertices[w];
					}//if
				}//for
			}//while
		}//if
	}//for
}//BFSTraverse
 
void main()
{
	ALGraph G;
	CreateGraph(G);
	list(G);
	//深度优先遍历图
	cout << "深度优先遍历：";
	DFSTraverse(G);
	cout << endl;
	//广度优先遍历
	cout << "广度优先遍历：";
	BFSTraverse(G);
	system("pause");
}
```

