---
title: ID3DX11EffectUnorderedAccessViewVariable SetUnorderedAccessViewArray method
description: Set an array of unordered-access-views.
ms.assetid: '12d0da06-990a-42b2-9566-cc5136f48781'
keywords: ["SetUnorderedAccessViewArray method Direct3D 11", "SetUnorderedAccessViewArray method Direct3D 11 , ID3DX11EffectUnorderedAccessViewVariable interface", "ID3DX11EffectUnorderedAccessViewVariable interface Direct3D 11 , SetUnorderedAccessViewArray method"]
topic_type:
- apiref
api_name:
- ID3DX11EffectUnorderedAccessViewVariable.SetUnorderedAccessViewArray
api_location:
- N/A
- N/A.dll
api_type:
- COM
---

# ID3DX11EffectUnorderedAccessViewVariable::SetUnorderedAccessViewArray method

Set an array of unordered-access-views.

## Syntax


```C++
HRESULT SetUnorderedAccessViewArray(
  �ID3D11UnorderedAccessView **ppResources,
  �UINT ���������������������Offset,
  �UINT ���������������������Count
);
```



## Parameters

<dl> <dt>

*ppResources* 
</dt> <dd>

Type: **[**ID3D11UnorderedAccessView**](id3d11unorderedaccessview.md)\*\***

An array of [**ID3D11UnorderedAccessView**](id3d11unorderedaccessview.md) pointers.

</dd> <dt>

*Offset* 
</dt> <dd>

Type: **[**UINT**](https://msdn.microsoft.com/library/windows/desktop/aa383751)**

Index of the first unordered-access-view.

</dd> <dt>

*Count* 
</dt> <dd>

Type: **[**UINT**](https://msdn.microsoft.com/library/windows/desktop/aa383751)**

Number of elements in the array.

</dd> </dl>

## Return value

Type: **[**HRESULT**](455d07e9-52c3-4efb-a9dc-2955cbfd38cc)**

Returns one of the following [Direct3D 11 Return Codes](d3d11-graphics-reference-returnvalues.md).

## Remarks

> [!Note]  
> The DirectX SDK does not supply any compiled binaries for effects. You must use Effects 11 source to build your effects-type application. For more information about using Effects 11 source, see [Differences Between Effects 10 and Effects 11](d3d11-graphics-programming-guide-effects-differences.md).

�

## Requirements



|                    |                                                                                                                                              |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| Header<br/>  | <dl> <dt>D3dx11effect.h</dt> </dl>                                                    |
| Library<br/> | <dl> <dt>N/A (An Effects 11 library is available online as shared source.)</dt> </dl> |



## See also

<dl> <dt>

[ID3DX11EffectUnorderedAccessViewVariable](id3dx11effectunorderedaccessviewvariable.md)
</dt> </dl>

�

�




