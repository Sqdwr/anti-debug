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
```
