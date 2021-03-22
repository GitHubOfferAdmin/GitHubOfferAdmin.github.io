次
世
代
C
语
言

C language-Next generation



登记号:国作登字-2021-A-01233328

Contact email:1091251059@qq.com


核心部分:

智能格式化内存函数与递归卸载内存函数.
格式扩展写法-乙类型.
多态的实现-扩展功能.
基础
基础-标准-c++书写方式.
多重扩展.
内存自动回收.





智能格式化内存函数与递归卸载内存函数:(入门结束)
‘.h’ +.c’构架,分别新建文本文件Boss.h Boss.c
.h中:
#ifndef Boss_H_
#define Boss_H_
struct Boss_
{
int id;
int hp;
int mp;
int lvl;char *name;
};typedef struct Boss_   Boss;
void _Boss_(void* ooobj);//声明格式化函数
void _$Boss_(void* ooobj);//声明递归卸载函数
#endif
.c中
void _Boss_(void* ooobj)//
{
Boss* tthis=(Boss*)ooobj;
tthis->name=(char*)malloc(sizeof(char)*100);	//A构造//暂时
}

void _$Boss_(void* ooobj)
{
Boss* tthis=(Boss*)ooobj;
if(tthis->name)
free(tthis->name);								//A删除

}
使用:
Boss* boss;                              
boss=0;
boss=(Boss*)malloc(sizeof(Boss));
_Boss_(boss); //格式化内存                
...............                               
_$Boss_(boss);  //递归卸载
free(boss);
boss=0;                                 






格式扩展写法-甲类型:(此处的typedef书写方式来源于网络)
#define Body_Kernel\
int id;\
int hp;\
int mp;\
int lvl;\
int life;
struct Body				struct Boss						struct Dragon
{							{								{
Body_Kernel				Body_Kernel					Body_Kernel	
							int pover;						int isFly;
int item;						int color;
};							};								};

_Body_(void*oobj);		_Boss_(void*oobj);	_Dragon_(void*oobj);
_$Body(void*oobj);		_Boss_(void*oobj);	_$Dragon_(void*oobj);
.c中:
_Body_(void*oobj)		_Boss_(void*oobj)		_Dragon_(void*oobj)
{							{						{
........						.........					...........
_$Body(void*oobj)		_Boss_(void*oobj)		_$Dragon_(void*oobj)
{...........					{............				{...........
格式扩展写法-乙类型:
新建纯虚文件 BodyAbs.h
int id;
int hp;
int mp;
int lvl;
int life;
本质高于法规.
struct Body				struct Boss						struct Dragon
{							{								{
#include“BodyAbs.h”		#include“BodyAbs.h”		#include“BodyAbs.h”
							int pover;						int isFly;
int item;						int color;
};							};								};
_Body_(void*oobj);		_Boss_(void*oobj);	_Dragon_(void*oobj);
_$Body(void*oobj);		_Boss_(void*oobj);	_$Dragon_(void*oobj);
.c中:
_Body_(void*oobj)		_Boss_(void*oobj)		_Dragon_(void*oobj)
{							{						{
........						.........					...........
_$Body(void*oobj)		_Boss_(void*oobj)		_$Dragon_(void*oobj)
{...........					{............				{...........






增强功能悖论:
扩展功能时的多态问题:
struct Stream				struct Socket					struct FileOut
{							{								{
Body_Kernel				Body_Kernel					Body_Kernel
						char* name;					int pos;
int ip;							char* dir;
};							};								};
char* load_Stream(		char* load_Socket(		char* load_FileOut(
void*,int);					void*,int);					void*,int);

应用:
void* IoData(Stream* stream)
{
char*data=0;
data=load_Socket(stream,128);			//不确定是否为socket
data=load_FileOut(stream,128);			//不确定是否为fileOut
data=load_MemSet(stream,128);			//不确定是否为memSet

};
目标样式:
stream->load();
多态的实现-扩展功能:
#define Stream_Kernel\
Body_Kernel\
char* (*load)(void* ooobj,int size);
struct Stream				struct Socket					struct FileOut
{							{								{
Stream_Kernel			Stream_Kernel				Stream_Kernel
						char* name;					int pos;
int ip;							char* dir;
};							};								};
char* load_Stream(		char* load_Socket(		char* load_FileOut(
void*,int);					void*,int);					void*,int);
基类初始化函数:
void _Stream_(void* ooobj,char*(*lload)(void*,int))
{
Stream* tthis=(Stream*)ooobj;
_Body_(ooobj);
tthis->load=lload;
}
基类卸载函数:
void _$Stream_(void* ooobj)
{_$Body_(ooobj)}
扩展类书写:
.c
char* load_Socket(void* ooobj,int size)
{
}
void _Socket_(void* ooobj)
{
Socket* tthis=(Socket*)ooobj;
_Stream_(ooobj,load_Socket);//居于句首
}
void _$Socket_(void* ooobj)
{
Socket* tthis=(Socket*)ooobj;
_$Stream_(ooobj);//居于句尾
}

.h
char* load_Socket(void* ooobj,int size);//声明

void _Socket_(void* ooobj);//声明

void _$Socket_(void* ooobj);//声明
应用:
	Stream* stream=0;
stream=CreateFileOut();
stream->load((void*)stream,512);
Stream* stream=0;
stream=CreateSocket();
stream->load((void*)stream,512);
void loadData_Boss(void*ooobj,Stream* stream);
{
Boss* tthie=(Boss*)ooobj;
char* bossData=0;
bossData=stream->load((void*)stream,512);
}









虚拟造物的基础:
#define Body_Kernel\
int type;\
void(*_$ClasObj_)(void* ooobj);
#define DoDeath(ooobj)\
ooobj->_$ClasObj_((void*)ooobj);\
free(ooobj);
Struct Body
{
Body_Kernel
};
void _Body_(void* oobj,void(*_$ClasObj_)(void* ooobj))
{
Body* tthis=(Body*)oobj;
tthis->_$ClasObj_=_$ClasObj_;
}
void _$Body_(void* oobj)
{
Body* tthis=(Body*)oobj;
}


Struct Boss			Struct Socket					Struct FileOut
{						{								{
Body_Kernel			Body_Kernel					Body_Kernel
.....						......							......
};						};								};
void _Socket_(void* oobj)
{
Body* tthis=(Body*)oobj;
_Body_(oobj,_$Socket_);
}
void _$Socket_(void* oobj){_$Body_(oobj);}
Body* CreateSocket()
{
Socket* tthis=(Socket*)malloc(sizeof(Socket));
_Socket_((void*)tthis);
return (Body*)tthis;
}
Body* body=CreateSocket();			//开始
DoDeath(body);						//终结
数据块扩展须居首位。核心删除函数位于Body_Kernel
Body_Kernel必须位于第一行。如Body_Kernel不居首DoDeath找不到正确的卸载函数。
虚拟造物的基础-标准c++书写格式:
.h
namespace ClasTypeCPushPush
{
	struct ClasObj_;typedef struct ClasObj_ ClasObj;
struct ClasObj_
{
Body_Kernel
};
ClasObj* Create();
void _ClasObj_(void* oobj);
void _$ClasObj_(void* oobj);
};
.cpp
namespace ClasTypeCPushPush
{
	ClasObj* Create()
{
ClasObj* tthis=(ClasObj*)malloc(sizeof(ClasObj));
memset(tthis,0,sizeof(ClasObj));
_ClasObj_((void*)tthis);
return tthis;
}
};
void _ClasObj_(void* oobj)
{
ClasObj* tthis=(ClasObj*)oobj;
_Body_(oobj,_$ClasObj_);
}
void _$ClasObj_(void* oobj)
{
ClasObj* tthis=(ClasObj*)oobj;
_$Body_(oobj);
}
实现:
ClasTypeCCplshPlush:: ClasObj* cpp;//申请指针
cpp=0;
cpp=ClasTypeCCplshPlush:: Create();//申请内存
DoDeath(cpp);
cpp=0;                        //删除,指针擦干净




双基类:
错误实例:
struct Animal					struct FileIo
{								{
Animal_Kernel					FileIo_Kernel
};								};
struct Dragon
{
Animal_Kernel	
FileIo_Kernel
};
Dragon* dragon=0;
dragon=CreateDragon();
dragon->Save((void*)dragon,”c:/dragonData.txt”);

扩展类型本质只是元素叠堆。上写法展开是
struct Dragon
{int type;
void(*_$ClasObj_)(void* ooobj);
int type;
void(*_$ClasObj_)(void* ooobj);};
类型只是看起来像个类型，本质还是一堆一次元数据。
多重扩展:
虚类型:
#define FileIo_Kernel\
void* FileIo;\
void (*save)(void* ooobj);
段数据
struct FileIo
{
FileIo_Kernel		//不存在Body的虚无数据集合
};
段初始化:
void _FileIo_(void*tthis,void* sourcsObj)
{
FileIo*tthis=(FileIo*)tthis;
tthis->FileIo=sourcsObj;			//A
}
段删除
void _$FileIo_(void*tthis,void* sourcsObj)
{
FileIo*tthis=(FileIo*)tthis;
}
核心函数:
void Save_FileIo(void*tthis)
{
FileIo*tthis=(FileIo*)tthis;
}

很明显,无法用DoDeath()
结合:
struct Dragon
{
Animal_Kernel	//根
Int x,y,z;
FileIo_Kernel//叶
}; 
void _Dragon_(void* ooobj)
{
Dragon* tthis=(Dragon* )ooobj;
_Animal_(ooobj);
_FileIo_((void*)&(tthis->FileIo),(void*)tthis);			//B
}
&(tthis->FileIo)		为段首地址用来初始化段
(void*)tthis			为句首



void _$Dragon_(void* ooobj)
{
Dragon* tthis=(Dragon* )ooobj;
.
.
.
_$Animal_(ooobj);
_$FileIo_((void*)&(tthis->FileIo));	
}
void Save_Dragon(void*ooobj)		//扩展半函数
{
Dragon*tthis=(Dragon*)((FileIo*)(ooobj)->FileIo);//C
Save_Dragon(ooobj);					//虚基类函数
}
应用:
Dragon* dragon=0;
dragon=CreateDragon();
dragon->Save((void*)&(dragon->FileIo));//D
DoDeath(dragon);
dragon=0;
内存自动回收:
.h
struct World
{
void* dataPointNum_World;//A-1
int* intArray;
char* woldName;
void* classObjBum_Wold;//A-2
Dragon* dragon;
Bosa* boss;
};
.c
void _Wold_(void* ooobj)
{
Wold* tthis=(Wold*)ooobj;
(tthis->dataPointNum_World)=(void*)2;//A_2
(tthis->ClasObjNum_World)=(void*)2;//B_2
}
void _$Wold_(void* ooobj)
{
Wold* tthis=(Wold*)ooobj;
void** oobjArray=(void**)(tthis->ClasObjNum_World);
oobjArray++;
Int llenth=0;//long llentg=0;
void* iiii=0;//intptr_t iiii=0;
iiii=(void*)(tthis->ClasObjNum_World);
llenth=(int)iiii;
int i=0;
for(i=0;i<llenth;i++)
{
Body* oobject=(Body*)oobjArray[i]
if(oobject)
DoDeath(oobject);
Oobject=0;
}


void** dataArray=(void**)(tthis->dataPointNum_World);
dataArray++;
int llenth=0;
void* iiii=0;
iiii=(void*)(tthis->dataPointNum_World);
llenth=(int)iiii;
int i=0;
for(i=0;i<llenth;i++)
{
void* oobject=(void*)dataArray[i]
if(dataArray)
frre(dataArray);
dataArray=0;
}



}













随着时间的流逝，语言一直在进化。从处理到逻辑，从低级到高级，从单元到多元。
第五代语言是语言建模的下一代标准，也是于计算机世界中顶点的炼金石。它有很多高级功能。可以在瞬间建立事物的抽象模型。
本资料是最基础的一册，讲述了本应在2000年左右问世的c语言进化阶段。它可以帮助我们跳出固定思维模式。无视规则。本资料上的所有实现高级语言的技巧方法只能用于教育教学，禁止用在其他方面。
