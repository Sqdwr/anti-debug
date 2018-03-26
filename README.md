# BoolDebug
- Judge Debug
- 代码来自书籍《有趣的二进制》
- 学校破网上传一直失败，我在这直接拷贝代码上来好了

#include <iostream>
#include <Windows.h>
using namespace std;

int TempEsp;

int BoolDebug0()
{
	return IsDebuggerPresent();
}

int BoolDebug1()
{
	BOOL t;
	CheckRemoteDebuggerPresent((HANDLE)-1, &t);
	return t;
}

int __declspec(naked) BoolDebug2()
{
	__asm
	{
		pushad;
		push ok;
		push dword ptr fs : [0];
		mov dword ptr fs : [0], esp;
		mov TempEsp, esp;
		push 100h;
		popf;
		jmp error;
	ok:
		mov esp, TempEsp;
		pop dword ptr fs : [0];
		add esp, 4;
		popad;
		xor eax, eax;
		ret;
	error:
		mov esp, TempEsp;
		pop dword ptr fs : [0];
		add esp, 4;
		popad;
		xor eax, eax;
		inc eax;
		ret;
	}
}

int __declspec(naked) BoolDebug3()
{
	__asm
	{
		pushad;
		push ok;
		push dword ptr fs : [0];
		mov dword ptr fs : [0], esp;
		mov TempEsp, esp;
		xor eax, eax;
		int 2dh;								//这个代码似乎是有问题的，因为这里的int 2d中断最终的处理就是扔给了kitrap03，如果存在编译器，这里会直接让编译器来处理int 3会出现一个断点
		jmp error;
	ok:
		mov esp, TempEsp;
		pop dword ptr fs : [0];
		add esp, 4;
		popad;
		xor eax, eax;
		ret;
	error:
		mov esp, TempEsp;
		pop dword ptr fs : [0];
		add esp, 4;
		popad;
		xor eax, eax;
		inc eax;
		ret;
	}
}

int main()
{
	BOOL tFlag;

	tFlag = BoolDebug3();

	if (tFlag)
		MessageBoxA(GetDesktopWindow(), "当前被调试！", "", MB_OK);
	else
		MessageBoxA(GetDesktopWindow(), "当前未被调试！", "", MB_OK);

	system("pause");
	return 0;
}
