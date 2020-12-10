---
title: 图（Floyd算法）
tags:
  - C
  - data structure
  - Algorithm
  - Graph

categories: 数据结构
comments: false
---

教材：严版数据结构

页码：P191-192

实现：算法7.16（解析：图7.37）

代码如下：

<!--more-->

```c
#include<stdio.h>
#include<iostream>
#include<stdlib.h>
#include<limits.h>
#include<iomanip>
 
using namespace std;
 
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define OVERFLOW -2
#define INFINITY 32767 //最大值
#define MAX_VERTEX_NUM 20 //最大顶点个数
typedef int Status;
typedef int VRType;
typedef int InfoType;
typedef bool*** PathMatrix;
 
 
//{有向图，有向网，无向图，无向网}
typedef enum { DG, DN, UDG, UDN }GraphKind;
 
typedef struct ArcCell {
	VRType adj;//VRType是顶点关系类型。对无权图，用1或0
			   //表示相邻否；对带权图，则为权值类型
	InfoType *info;//该弧相关信息的指针
}ArcCell, AdjMatrix[MAX_VERTEX_NUM][MAX_VERTEX_NUM];
 
typedef ArcCell** DistancMatrix;
typedef char VertexType;
typedef struct {
	VertexType vexs[MAX_VERTEX_NUM];//顶点向量
	AdjMatrix arcs;//邻接矩阵
	int vexnum, arcnum;//图的当前顶点数和弧数
	GraphKind kind;//图的种类标志
}MGraph;
 
 
 
 
//在G中找到v对应的顶点位置
int LocateVex(MGraph G, char v)
{
	int i;
	for (i = 0; i < G.vexnum; i++)
	{
		if (G.vexs[i] == v)
		{
			return i;
		}
	}
	return -1;
}
 
 
 
Status CreateDN(MGraph &G)
{
	int i, j, k, w;
	VertexType v1, v2;
	cout << "输入顶点数G.vexnum:";
	cin >> G.vexnum;
	cout << "输入边数G.arcnum:";
	cin >> G.arcnum;
	getchar();
	for (i = 0; i < G.vexnum; i++)
	{
		cout << "输入顶点G.vexs[" << i << "]" << endl;
		cin >> G.vexs[i];
		getchar();
	}//构造顶点向量
 
	 //初始化邻接矩阵
	for (i = 0; i < G.vexnum; i++)
	{
		for (j = 0; j < G.vexnum; j++)
		{
			G.arcs[i][j].adj = INFINITY;
			G.arcs[i][j].info = NULL;
			if (i == j)
			{
				G.arcs[i][j].adj = 0;
			}
		}
	}
	//构造邻接矩阵
	for (k = 0; k < G.arcnum; ++k)
	{
		cout << "输入第" << k + 1 << "条边vi、vj和权值w(int):" << endl;
		//输入一条边依附的顶点及权值
		cin >> v1;
		cin >> v2;
		cin >> w;
		getchar();
		//确定v1和v2在G中的位置
		i = LocateVex(G, v1);
		j = LocateVex(G, v2);
		G.arcs[i][j].adj = w;//弧<v1,v2>的权值
	}
	return OK;
}
 
void list(MGraph G)
{
	int i, j;
	cout << "输出邻接矩阵：" << endl;
	for (i = 0; i < G.vexnum; ++i)
	{
		cout << G.vexs[i] << "----";
		for (j = 0; j < G.vexnum; ++j)
		{
			if (G.arcs[i][j].adj == INFINITY)
				cout << setw(4) << "∞";
			else
				cout << setw(4) << G.arcs[i][j].adj;
		}
		cout << endl;
	}
}
 
void ShortestPath_FLOYD(MGraph G,PathMatrix &P, DistancMatrix &D)
{
	int i, j, v, w,u;
	P = (bool ***)malloc(sizeof(bool *)*G.vexnum);
	for (i = 0; i < G.vexnum; i++)
		P[i] = (bool **)malloc(sizeof(bool)*G.vexnum);
	for (i = 0; i < G.vexnum; i++)
		for(j=0;j<G.vexnum;++j)
		P[i][j] = (bool *)malloc(sizeof(bool)*G.vexnum);
 
	D = (ArcCell **)malloc(sizeof(ArcCell)*G.vexnum);
	for (i = 0; i < G.vexnum; ++i)
	{
		D[i]= (ArcCell *)malloc(sizeof(ArcCell)*G.vexnum);
	}
	
	//各对结点之间初始已知路径及距离
	for (v = 0; v < G.vexnum; ++v)
	{
		for (w = 0; w < G.vexnum; ++w)
		{
			D[v][w] = G.arcs[v][w];
			for (u = 0; u < G.vexnum; ++u)
			{
				P[v][w][u] = FALSE;
			}
			//从v到w有直接路径
			if (D[v][w].adj < INFINITY&&D[v][w].adj!=0)
			{
				P[v][w][v] = TRUE;
				P[v][w][w] = TRUE;
			}//if
		}//for
	}
	
	for (u = 0; u < G.vexnum; ++u)
	{
		for (v = 0; v < G.vexnum; ++v)
		{
			for (w = 0; w < G.vexnum; ++w)
			{
				//从v经u到w的一条路径更短
				if (D[v][u].adj + D[u][w].adj < D[v][w].adj)
				{
					D[v][w].adj = D[v][u].adj + D[u][w].adj;
					for (i = 0; i < G.vexnum; ++i)
					{
						P[v][w][i] = P[v][u][i] || P[u][w][i];
					}
				}//if
			}
		}
	}
 
	cout << "输出每一对顶点之间的最短路径长度如下：" << endl;
	for (v = 0; v < G.vexnum; ++v)
	{
		cout << G.vexs[v] << "----";
		for (w = 0; w < G.vexnum; ++w)
		{
			if (D[v][w].adj == INFINITY)
				printf("%4s", "∞");
			else
				printf("%4d", D[v][w].adj);			
		}
		cout << endl;
	}
 
	
	
}
 
void main()
{
	MGraph G;
	int v0;
	PathMatrix P;
	DistancMatrix D;
	CreateDN(G);
	list(G);
	ShortestPath_FLOYD(G, P, D);
	cout << endl;
	system("pause");
}
 
```

