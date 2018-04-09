```C
void __declspec(naked) Mix()
{
	_asm
	{
		_emit 0xEB;
		_emit 0xFF;						//JMP -1之后执行FF C0(inc eax)，48（dec eax）
		_emit 0xC0;
		_emit 0x48;

		ret;
	}
}

void __declspec(naked) Mix2()
{
	_asm
	{
		_emit 0x66;
		_emit 0xB8;
		_emit 0xEB;
		_emit 0x05;			
		_emit 0x31;
		_emit 0xC0;
		_emit 0x74;
		_emit 0xFA;
		_emit 0xE8;
		/*第一次运行是66 B8 E8 05(mov ax,0x05EB),31 C0(xor eax),74 FA(jz -6)跳转到0xEB
		第二次运行是EB 05
		直接跳转到ret指令的位置
		*/

		ret;
	}
}
```
