---
title: 'How to extend Oasis Montaj using C#'
author: sergio
type: post
date: 2011-05-10T15:34:19+00:00
url: /2011/05/10/how-to-extend-oasis-montaj-using-c/
categories:
  - programación

---
Oasis Montaj allows its users to program their own extensions. The &#8220;standard&#8221; way of creating an Oasis Montaj extension is by using the [GX language][1], a C-like language, and its companion user interface definition language. I find this good enough for most cases. Actually, the standard actions in Oasis Montaj seem to be implemented as GX scripts that validate user input and then calls some inner functionality.

However, for more complex programs, I like to have all the bells and whistles of a main stream high level language, such as C#. In this post I will explain step by step how to create a &#8220;Hello World&#8221; C# Oasis Montaj extension.

I will be using Microsoft Visual C# 2010 express, you can download for free [here][2]. You will also need to have a licensed copy of Oasis Montaj installed (I&#8217;m using version 7.2.1). Finally, you&#8217;ll need the corresponding GX developer toolkit that you can download for free [here][3].

1. Create a **new project** in Visual C# 2010 express.  Choose **class library** as the type of project and name it **HelloWorld**, for example.

2. Save the project.

3. Go to the project properties (Project -> HellowWorld Properties) and change the **Target framework** to **.NET framework 3.5**. This is the version that seem to work well for Oasis Montaj 7.2.1, but I guess it might change in the future. Anyway, version 4.0 does not seem to work. **UPDATE**: starting from Oasis Montaj 7.3, the target platform should be set to .NET framework 4.0. Yay!

4. In the **Solution Explorer** tab, right-click the HelloWorld project and then choose **Add Reference**. Select **Browse** in the dialog window and then navigate to the folder where you installed GX Developer (usually C:Program FilesGeosoftGX Developer). Then navigate through the folders apps -> redist -> bin. Select **geonet.dll** and **Geosoft.GX.Controls.dll**

5. Repeat the previous step to add a new reference, but instead of selecting the Browse tab, select **.NET**, then add **System.Windows.Forms**

6. Create a new class named  HelloWorld:

<pre class="brush: csharp; title: ; notranslate" title="">using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Geosoft.GX.Controls;
using Geosoft.GXNet;
using System.Windows.Forms;

namespace HelloWorldGX
{
class HelloWorldGX : BaseForm
{
public HelloWorldGX(int i) : base(i) { InitializeForm(); }
public int Run() {
MessageBox.Show("Hello World");
return 0;
}
}
}

</pre>

7. In Visual C#, click on Build -> Build Solution. Then go to your project folder -> bin -> Release, and copy the HelloWorld.dll to the **bin** folder of your Oasis Montaj install

8. Create a new menu file named **HelloWorld.omn** in the omn folder of your Oasis Montaj install folder (C:Program GilesGeosoftOasis Montaj, usually) with the following content:

<pre class="brush: plain; title: ; notranslate" title="">MENU "&HelloWorld" ITEM "&HelloWorld" ,HelloWorld.dll(HelloWorldGX.HelloWorldGX;Run) &lt;standard.bmp[1]&gt; </pre>

9. Copy the **geonet.dll**, **Geosoft.GX.Controls.dll** and **geoexceptions.dll from the GX Developer -> apps -> redist -> bin** to the **bin folder of your Oasis Montaj install.** Important note: you might need to copy other dll&#8217;s when you start using some real functions from the oasis montaj API.**
  
** 

10. From Oasis Montaj, load the HelloWorld Menu (GX -> Load Menu), and then run the Hello World item.**
  
** 

At [Abitibi Geophysics][4] we have been writing Oasis Montaj&#8217;s extensions using C# with very good results.

 [1]: http://www.geosoft.com/downloads/whatisit/gxdevtools.asp "oasis montaj gx toolkit"
 [2]: http://www.microsoft.com/express/downloads/#2010-Visual-CS "visual c#"
 [3]: http://www.geosoft.com/support/devtools/index.asp "GX developer toolkit"
 [4]: http://www.ageophysics.com/ "Abitibi Geophysics"