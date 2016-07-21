# csi
This projects extends the [Visual Studio CSharp Interactive REPL](https://github.com/dotnet/roslyn/wiki/Interactive-Window) to provide fancy Matlab - like syntax and features. 

This repository hosts a script for the Visual Studio interactive CSharp console REPL (CSI). It provides convenience functions to be used inside the REPL and to simulate a look-and-feel similar to other popular mathematical prototyping systems, namely Matlab[R] and Octave.

ILNumerics CSI is a community project. It can be used by everyone free of charge. Please help to extend and maintain this project! We are happy to accept pull requests! 

# Requirements
* Visual Studio 2015, Update 1 or later
* ILNumerics 4.12 or later ([download](http://ilnumerics.net/download)). Note that older version of ILNumerics will require a trial license to be valid on the system.

# Getting started
We use the *unmodified* CSI REPL. To make the new features available download a script from this repository and load it into the CSI. The necessary steps are explained below.

1. Start Visual Studio and open the CSharp interactive REPL (CSI). The REPL is available in all editions of Visual Studio 2015, Update 1 or later. Go: VIEW -> Other Windows -> C# Interactive.  
2. Now load the script from this git repository. You can do this directly from the REPL! Just use the `WebClient.DownloadFile` method. The CSI supports us in including the neccessary `using` directive. When we start typing...
    new WebClient() 
it displays a light bulb which we can use to add the suggested `using System.Net` namespace. 

![CSI Light Bulb](http://ilnumerics.net/media/png/CSI_lightbulb.png)

The full command to load the script will look like this:
     > using System.Net;
     . 
     . new WebClient().DownloadFile("https://raw.githubusercontent.com/ILNumerics/csi/master/ilnumerics.rsp", "ilnumerics.rsp");
    
You can simply copy&paste these lines into the CSI REPL and press `Enter`. This will download the ILNumerics script from the GIT repository and store it into your user's directory.

3. Run the script in the REPL: 
     #load "ilnumerics.rsp"

# First Steps
Make all functions from ILMath directly available: 

     using static ILNumerics.ILMath; 

Plot some numbers: 

     plot(new double[] {1,3,2,4,-2,-3,10,0.4});
![CSI Plot1](http://ilnumerics.net/media/png/CSI_Plot1.png)

Plot some more numbers, use the same figure:

     plot(randn(100,10000));
![CSI Plot2](http://ilnumerics.net/media/png/CSI_Plot2.png)

Give the figure a name: 

     figure().Scene.Add(new ILTitle("my Plot is Hot"))

Move the title with the left mouse button: 
![CSI Plot3](http://ilnumerics.net/media/png/CSI_Plot3.png)

