Built-in methods allows your mod to understand what the heck is going on in the game.

The first one we will go over is called ```OnModLoaded()```. Modify your code to look like this:
```csharp
using ModdingToolsBase;
namespace ModTest1
{
    public class Mod : ModdingTool
    {
        public override void OnModLoaded()
        {
            
        }
    }
}
```
When your mod is loaded, the method is ran. We can add
```ModLogger.LogMessage("My Mod is loaded!");``` to log text when the mod is loaded into the console. We can also set the name of our mod using ```ModInfo.ModName = "MyMod";```. Our code should look like this:
```csharp
using ModdingToolsBase;
namespace ModTest1
{
    public class Mod : ModdingTool
    {
        public override void OnModLoaded()
        {
            ModInfo.ModName = "MyMod";
            ModLogger.LogMessage("My Mod is loaded!");
        }
    }
}
```
This will print in the console:
![[Pasted image 20240101155528.png]]

The next method that's built-in is called ```OnSetupRound()```. This method is ran on ALL the mods, even if it has not been selected for this round! I highly recommend NOT putting any ModLogger statements in here, as it will clog up the console.

Next, we have `OnAgentSelected()`. This code is ran when it is guaranteed that this agent will be in the next round. The round has not started yet.

Next, we have `OnRoundStart()`. This runs when the round is started.
If we add all of this, our code should look something like this:
```csharp
using ModdingToolsBase;
namespace ModTest1
{
    public class Mod : ModdingTool
    {
        public override void OnModLoaded()
        {
            ModInfo.ModName = "MyMod";
            ModLogger.LogMessage("My Mod is loaded!");
        }
        public override void OnSetupRound()
        {
            //Runs this code when a round is started. Try NOT to put Log messages here!
        }
        public override void OnAgentSelected()
        {
            //Runs this code before the round has started but after the agent is guarenteed to be in the round.
            ModLogger.LogMessage("This agent has been selected!");
        }
        public override void OnRoundStart()
        {
            //Runs this code when a round is started
            ModLogger.LogMessage("Round has been started with current agent");
        }
    }
}
```

Now we are getting to the important stuff, this is where our agent decides whether to cooperate or not.
When the game asks for the decision, it will run `OnPickingTime(ResultChoice choiceOptions)`. This override needs to be a ResultChoice, and not a void. Here is an example that no matter what, will always cooperate:
```csharp
public override ResultChoice OnPickingTime(ResultChoice choiceOptions)
{
    //Do logic for whether we should pick or not
    choiceOptions.choice = ToDoWhenReady.Cooperate;
    return choiceOptions;
}
```

The game will then compare the values between the two mods, and send out rewards accordingly. This method should be called 200 times, and after we receive the results, we will run `OnSmallRoundFinished(SmallRoundResult result)`. Result contains info about the points we gained, the choice the other player made, and the finished round. A filled method may look like this:
```csharp
public override void OnSmallRoundFinished(SmallRoundResult result)
{
    //Do logic for what to do when the small round finishes
    ModLogger.LogMessage($"The results are in! We gained a total of {result.totalPointsEarned} points!");
}
```

Finally, when all the rounds have ended, the game will run `OnGameEnded(TotalGameResults results)`. This will provide info on who won, and how many points each player obtained.

A completed agent that always chooses to cooperate may look like this:
```csharp
using ModdingToolsBase;
namespace ModTest1
{
    public class Mod : ModdingTool
    {
        public override void OnModLoaded()
        {
            ModInfo.ModName = "MyMod";
            ModLogger.LogMessage("My Mod is loaded!");
        }
        public override void OnSetupRound()
        {
            //Runs this code when a round is started. Try NOT to put Log messages here!
        }
        public override void OnAgentSelected()
        {
            //Runs this code before the round has started but after the agent is guarenteed to be in the round.
            ModLogger.LogMessage("This agent has been selected!");
        }
        public override void OnRoundStart()
        {
            //Runs this code when a round is started
            ModLogger.LogMessage("Round has been started with current agent");
        }
        public override ResultChoice OnPickingTime(ResultChoice choiceOptions)
        {
            //Do logic for whether we should pick or not
            choiceOptions.choice = ToDoWhenReady.Cooperate;
            return choiceOptions;
        }

        public override void OnSmallRoundFinished(SmallRoundResult result)
        {
            //Do logic for what to do when the small round finishes
            ModLogger.LogMessage($"The results are in! We gained a total of {result.totalPointsEarned} points!");
        }

        public override void OnGameEnded(TotalGameResults results)
        {
            //Do logic for what to do when the full round ends
            string toSay = "";
            if (results.DidPlayerWin)
            {
                toSay = "We won!";
            }
            else
            {
                toSay = "We lost.";
            }
            ModLogger.LogMessage($"The game has ended! " + toSay);
        }
    }
}
```

Now you can move onto [[Adding your mod to the game]] or for more advanced usage tips, you can go to [[Internal Code]].