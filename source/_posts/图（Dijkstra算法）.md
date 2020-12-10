---
title: 图（Dijkstra算法）
tags:
  - C
  - data structure
  - Algorithm
  - Graph

categories: 数据结构
comments: false
---

教材：严版数据结构

页码：P189

实现：算法7.15

IDE：VS2015

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
typedef bool** PathMatrix;
 
 
//{有向图，有向网，无向图，无向网}
typedef enum { DG, DN, UDG, UDN }GraphKind;
 
typedef struct ArcCell {
	VRType adj;//VRType是顶点关系类型。对无权图，用1或0
			   //表示相邻否；对带权图，则为权值类型
	InfoType *info;//该弧相关信息的指针
}ArcCell, AdjMatrix[MAX_VERTEX_NUM][MAX_VERTEX_NUM];
 
typedef ArcCell* ShortPathTable;
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
 
void ShortestPath_DIJ(MGraph G, int v0, PathMatrix &P, ShortPathTable &D)
{
	int i, j, v, w;
	P = (bool **)malloc(sizeof(bool *)*G.vexnum);
	for (int i = 0; i < G.vexnum; i++)
		P[i] = (bool *)malloc(sizeof(bool)*G.vexnum);
	D = (ArcCell *)malloc(sizeof(ArcCell)*G.vexnum);
 
	bool *final = new bool[G.vexnum];
 
	for (v = 0; v < G.vexnum; ++v)
	{
		final[v] = FALSE;
		D[v] = G.arcs[v0][v];
		for (w = 0; w < G.vexnum; ++w)
		{
			P[v][w] = FALSE;
		}
		if (D[v].adj < INFINITY)
		{
			P[v][v0] = TRUE;
			P[v][v] = TRUE;
		}
	}//for
 
	int min;
	//初始化,v0顶点属于S集
	D[v0].adj = 0; final[v0] = TRUE;
	//开始主循环，每次求得v0到某个顶点的最短路径，并加v到S集
	for (i = 1; i < G.vexnum; ++i)
	{
		min = INFINITY;
		for (w = 0; w < G.vexnum; ++w)
		{
			//w顶点在V-S中
			if (!final[w])
			{
				//w顶点离v0顶点更近
				if (D[w].adj < min)
				{
					v = w;
					min = D[w].adj;
				}
			}
		}
		final[v] = TRUE;//离v0顶点最近的v加入S集
		//更新当前最短路径和距离
		for (w = 0; w < G.vexnum; ++w)
		{
			//修改D[w]和P[w]
			if (!final[w] && (min + G.arcs[v][w].adj < D[w].adj))
			{
				D[w].adj = min + G.arcs[v][w].adj;
				P[w] = P[v];
				P[w][w] = TRUE;
			}
		}
 
 
	}
	
	for (i = 0; i < G.vexnum; ++i)
	{
		if(i!=v0)
		{
			printf("%3c  %3c  ",G.vexs[v0],G.vexs[i]);
			if (D[i].adj != INFINITY)
				printf("%5d    ", D[i]);
			cout << "(";
			for (j = 0; j < G.vexnum; ++j)
			{
				if (P[i][j])
					cout << G.vexs[j];
			}
			cout << ")";
			cout << endl;
		}
	}
}
 
void main()
{
	MGraph G;
	int v0;
	PathMatrix P;
	ShortPathTable D;
	CreateDN(G);
	list(G);
	cout << "输入源点序号v0:";
	cin >> v0;
	cout << "输出从源点" << v0 << "到其余顶点的最短路径如下：" << endl;
	cout << "始点 终点 路径长度 最短路径" << endl;
	ShortestPath_DIJ(G, v0, P, D);
	cout << endl;
	system("pause");
}
 
```

