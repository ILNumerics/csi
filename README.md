# csi
This projects extends the [Visual Studio CSharp Interactive REPL](https://github.com/dotnet/roslyn/wiki/Interactive-Window) to provide fancy Matlab - like syntax and features for ad-hoc evaluations and prototyping in Visual Studio. 

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

     plot(randn(1,10000));
![CSI Plot2](http://ilnumerics.net/media/png/CSI_Plot2.png)

Give the figure a name. (Use the CSI light bulb to import new namespaces! When the light bulb is shown just press `CTRL+.`)

     figure().Scene.Add(new ILTitle("my Plot is Hot"))

Move the title with the left mouse button: 
![CSI Plot3](http://ilnumerics.net/media/png/CSI_Plot3.png)

Use the left mouse to drag a zoom rectangle, double click resets the scene, right mouse is for panning. 

# Next steps
Until more higher-level functions have been implemented into ILNumerics-csi (the script in this repository) you can already use all of ILNumerics Computing Engine, Visualization Engine and Toolboxes by utilizing the [common API](http://ilnumerics.net/docs).

Add a new figure with 3D [plot cube](https://ilnumerics.net/plot-cube-properties.html): 

     var pc = figure(2).Scene.Add(new ILPlotCube(twoDMode: false));

In 3D the left mouse rotates the scene, scene reset and panning are done just like for 2D. 

Create an empty [fast surface](https://ilnumerics.net/interactive-fast-surface-visualization.html) and update it with example terrain data: 

     var sf = pc.Add(new ILFastSurface());
     sf.Update(Z: tosingle(ILSpecialData.terrain), colormap: Colormaps.Hot);
     // add a colorbar
     sf.Add(new ILColorbar()); 
     // dont forget the [obligatory for the low-level API] call to Configure()
     sf.Configure();
     // reset the plot cube limits
     figure().PlotCube.Reset();

![CSI Plot4](http://ilnumerics.net/media/png/CSI_Plot4.png)

Add a [down-scaled](https://ilnumerics.net/ilnumerics-interpolation-toolbox.html) version of the same data as [regular wireframed, lit surface](https://ilnumerics.net/surface-plots.html): 

     var sf2 = figure().PlotCube.Add(
        new ILSurface(
            interp2(tosingle(ILSpecialData.terrain),
                    Xn1: X1, Xn2: X1,
                    method: ILNumerics.Toolboxes.InterpolationMethod.spline) + 4000,
            X1, X1));
     sf2.UseLighting = true; 
     sf2.Configure(); 

Configure [axis label](https://ilnumerics.net/axis-configuration.html): 

     figure().PlotCube.Axes.XAxis.Label.Text = @"Distance \Delta_1 [km]" 

Handle [mouse events](https://ilnumerics.net/mouse-events.html). Here: [pick position](https://ilnumerics.net/apidoc/?topic=html/M_ILNumerics_Drawing_ILPanel_PickPrimitiveAt.htm) on upper surface and display the value in forms title bar: 

     figure().Panel.SceneSyncRoot.First<ILSurface>().Fill.MouseClick += (_s, _a) => {
        var pick = figure().Panel.PickPrimitiveAt(_a.Target as ILDrawable, _a.Location);
        if (pick != null && pick.VerticesWorld.S[0] > 2) {
            float s = pick.VerticesWorld.GetValue(0, 3);
            float v = pick.VerticesWorld.GetValue(1, 3);
            // The third barycentric coordinate is not stored. You can compute it easily 
            // using the following formula: 
            float t = 1 - s - v;
            // interpolate: here, we interpolate the position 
            ILArray<float> interp = pick.VerticesWorld["0;0:2;0"] * s
                              + pick.VerticesWorld["1;0:2;0"] * v
                              + pick.VerticesWorld["2;0:2;0"] * t;
            figure(2).Text = $"X:{interp.GetValue(0)} Y:{interp.GetValue(1)} Z:{interp.GetValue(2)}";
        }
     };
![CSI Plot5](http://ilnumerics.net/media/png/CSI_Plot5.png)

# More...

* Visit the [ILNumerics Examples section](https://ilnumerics.net/examples.php) to get more ideas what is possible with ILNumerics. Fetch your own [license](http://ilnumerics.net/download) and read more in the ILNumerics [online docunmentation](https://ilnumerics.net/docs.html). 

* Fork this repository and add new higher-level functions, similar to `plot` etc. Send pull requests and open new issues here on github. 

* Spread the word!
