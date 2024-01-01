To create a mod, you need to start by having .NET 7.0 installed. Then, you want to create a Class Library in Visual Studio. It should look like this:![[Pasted image 20240101153449.png]]
(Your class and namespace can be called whatever you want.)

Next, you want to add a dependency to the modding tools for the program. On the right of your code, you should see a solution explorer. Right click the "Dependencies" box and select "Add Project Reference."
![[Pasted image 20240101153617.png]]
Click Browse on the bottom right corner:
![[Pasted image 20240101153720.png]]
This will show a file explorer. Go to where the game is installed and select the file called "ModdingToolsBase.dll". Click Add, then click OK.

Now, add  ```using ModdingToolsBase``` to the top of your code. Then, make the class in your script to be derive from ```ModdingTool```. Your code should look like this:
```csharp
using ModdingToolsBase;
namespace ModTest1
{
    public class Mod : ModdingTool
    {

    }
}
```

You now have a base for making mods for the tool! Continue along with [[Built-in Methods]].