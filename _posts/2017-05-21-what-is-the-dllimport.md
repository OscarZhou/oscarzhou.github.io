---
title: "What is The DLLImport?"
date: 2017-05-21
category: C#
tags: [C#, DLL]

---


## Introduction

**DLLImport** is an attribute class in the namespace of `System.Runtime.InteropServices`, which is used for providing necessary calling information of the function exported by unmanaged DLL. Using DLLImport requires at least provide the name of DLL including entry point.  
  
**Explanation:**
  
* DLLImport only can be placed in the method declaration. The code below is where the `MessageBox` (located in User32.lib) is called.  
        
        #using <mscorlib.dll>
        using namespace System::Runtime::InteropServices; 
        // for DllImportAttribute
        
        namespace SysWin32
        {
           [DllImport("user32.dll", EntryPoint = "MessageBox", CharSet = Unicode)]
           int MessageBox(void* hWnd, wchar_t* lpText, wchar_t* lpCaption, 
                          unsigned int uType);
        }
        
        int main( )
        {
           SysWin32::MessageBox( 0, L"Hello world!", L"Greetings", 0 );
        }


* DLLImport has single located parameter(specifies the DLL name of exported method)  

* DLLImport has five named parameters:  

    1. **CallingConvention** specifies the calling convention of entry point. If there is not *CallingConvention*, the default value *CallingConvention.Winapi* will be used.  
    2. **CharSet** specifies the char sets of entry point. If it is not specified, the default value *CharSet.Auto* will be used.  
    3. **EntryPoint** offers the name of the DLL entry point. If it is not offered, the method name will be used.  
    4. **ExactSpelling** specifies whether the spelling of the *EntryPoint* must be as same as specified entry point. If it is not specified, the default value will be false.  
    5. **PreserveSig** specfies that the signature of the method should be reserved or transformed. When the signature is transformed, it is converted to the signature with a return value of `HRESULT` and a attached exported parameter that is named *retval*. If the *PreserveSig* is not specified, the default value will be true.  
    6. **SetLastError** specifies whether the last error of Win32 will be reserved. If the *SetLastError* is not specified, the default value will be false.  
    
* DLLImport is a one-time attribute class.  
        
        [DllImport("libvlc", CallingConvention = CallingConvention.StdCall, ExactSpelling = true)]
        [SuppressUnmanagedCodeSecurity]
        private static extern IntPtr libvlc_new(int argc, IntPtr argv);
        
        [DllImport("libvlc", CallingConvention = CallingConvention.StdCall, ExactSpelling = true)]
        [SuppressUnmanagedCodeSecurity]
        public static extern void libvlc_release(IntPtr libvlc_instance);
        
* The method modified by DLLImport must contain the `extern`.  


***

## How to use DLLImport?
In C#, you can declare it like this:

        [DllImport("a.dll")] 
        public extern static int F(); 
        
But when you call it, the system will prompt that <mark>No entry point can be found.</mark> When you use the command line `dumpbin /export a.dll`([How to check the name of the exported parameter of the DLL](#commandline)), you can find that the method name is not "F" instead "?F@@YAHXZ". So you need to change your declaration like below:  

        [DllImport("a.dll",EntryPoint="?F@@YAHXZ")]  
        public extern static int F();  
        
And then you can call it sucessfully!  


***
 
## Q&A


*Q: What is the difference between using DLLImport to import the DLL and adding DLL by references?*


A: DLLImport is a standard DLL, which can be developed by any programming language, like DELPHI, C++.  
Adding DLL by references is .NET DLL, non-standard DLL. It is only a class library.  


*Q: What is the mean of `[DLLImport("kernel32.dll")]`?*

A: This DLL library(kernel32.dll) contains many WindowsAPI function. If you want to use them, you should do like this:   
`[DLLImport("kernel32.dll")]`  
`private static extern void function_name(parameter1, [parameter2]);`  
*function_name* is a function in kernel32.dll. If you declare it like above, then you can call it directly. The calling operation is the same with typical calling operation. 

***

<h2 id="commandline">How to Use Command Line to Check the Name of the DLL Exported Parameters</h2> 

The prerequisites is that you have Visual Studio IDE in your computer. In my case, I use the Visual Studio 2013. Right click the VS icon and click the "Open the location of the file". You can see that there is a directory called "**Visual Studo Tools**". Then open the **Developer Command Prompt for VS2013**. And then you input the command like below:  

<p align="center">
  <img src="/images/post/20170521001.png" alt="Command Line to Check the Method Name" width="70%" /><br/>
  <center><h4><b>Figure 1</b></h4></center>
</p>

The format is `dumpbin /exports DLL's physical path`  


***

Hope that it can help you guys.  

