# 📋 Roblox World Ideas

- [ ] **House people.  Go shopping.  Play video games.  Have get-togethets in the town center each night.**

- [ ] **Each person should have two actions and 2 reactions**

- [ ] **Make it night and day.**

- [ ] **So four actions for normal people.  Wake up, go run jobs, then rest at house, then hang at town center, then sleep**

- [ ] **What other actors?**

- [ ] **Have them go to work?**

- [ ] **We need bandits**

- [ ] **Robbers, cops, normal, doctors.  Don't die, but when die you have to cripple back to the doctor**

- [ ] **What's the downside of people not doing jobs?  More bandits causing people to go to doctor, more bandits because police not doing their job.  People have to keep going to doctor, and not being able to work.  It's like sin city.  And if ships are not open, nothing fun to get.**

- [ ] **Well of bandits win, then game restarts.  If normal people win then they win.**

- [ ] **Have city money, and everyone makes money.**

- [ ] **While normal people work, they can solve problems.**

- [ ] **Harder the problem, more movney they get.**

- [ ] **Mad city has built it nice**

- [ ] **This is how to add and remove things**
  
  <details>
  <summary>💬 Comments (1)</summary>
  
  **Comment 1:** local Tornado = game.Workspace["Tornado by anphu04"]

wait(3)
Tornado.Parent = nil
wait(3)
Tornado.Parent = Workspace
  
  </details>

- [ ] **old bandit**
  
  <details>
  <summary>💬 Comments (1)</summary>
  
  **Comment 1:** ```-- Variables for the dummy and its humanoid
local bandit1 = game.Workspace.bandit1
local bandit1Humanoid = bandit1.Humanoid

-- Variables for the dummy and its humanoid
local civilian1 = game.Workspace.civilian1
local civilian1Humanoid = civilian1.Humanoid


-- Variables for the part
local checkPoint1 = game.Workspace.checkPoint1
local checkPoint2 = game.Workspace.checkPoint2
local checkPoint3 = game.Workspace.checkPoint3

-- Variables for the animation
local runAnimation = bandit1.RunAnim
local runAnimationTrack = bandit1Humanoid:LoadAnimation(runAnimation)
local walkAnimation = bandit1.WalkAnim
local walkAnimationTrack = bandit1Humanoid:LoadAnimation(walkAnimation)



function findNearestTorso(pos)
	--Notrd on this.  
	--NPCs always have a humaniod.  But players do not always have a fumaniod.  
	--So I need ot make this script better to handle both.  And prioritize the players over the dummy. 
	local list = game.Workspace:children()
	local torso = nil
	local dist = 1000000000000
	local temp = nil
	local human = nil
	local temp2 = nil
	for x = 1, #list do
		temp2 = list[x]
		if (temp2.className == "Model") and (temp2 ~= script.Parent) then
			temp = temp2:findFirstChild("HumanoidRootPart")
			human = temp2:findFirstChild("Humanoid")
			if (temp ~= nil) and (human ~= nil) and (human.Health > 0) then
				if (temp.Position - pos).magnitude < dist then
					torso = temp
					dist = (temp.Position - pos).magnitude
				end
			end
		end
	end
	return torso
end



function onTouched()
	--if not hit or not hit.Parent then return end
	--local human = hit.Parent:findFirstChild("Humanoid")
	--if human and human:IsA("Humanoid") then
		--human:TakeDamage(100)
	--end
	civilian1Humanoid:TakeDamage(100)
	
end


local function moveTo(humanoid, targetPoint, andThen)
	local targetReached = false

	-- listen for the humanoid reaching its target
	local connection
	connection = humanoid.MoveToFinished:Connect(function(reached)
		targetReached = true
		connection:Disconnect()
		connection = nil
		if andThen then
			andThen()
		end
	end)

	-- start walking
	humanoid:MoveTo(targetPoint)

	-- execute on a new thread so as to not yield function
	spawn(function()
		while not targetReached do
			-- does the humanoid still exist?
			if not (humanoid and humanoid.Parent) then
				break
			end
			-- has the target changed?
			if humanoid.WalkToPoint ~= targetPoint then
				break
			end
			-- refresh the timeout
			humanoid:MoveTo(targetPoint)
			wait(6)
		end

		-- disconnect the connection if it is still connected
		if connection then
			connection:Disconnect()
			connection = nil
		end
	end)
end


wait(1)

local target = findNearestTorso(script.Parent.HumanoidRootPart.Position)
if target ~= nil then
	runAnimationTrack:Play()
	bandit1Humanoid:MoveTo(target.Position, target.Position)
	bandit1Humanoid.MoveToFinished:Wait()
	--script.Parent.Touched:connect(onTouched)
	civilian1Humanoid:TakeDamage(100)
	runAnimationTrack:Stop()
	
end
	
wait(1)
  
  </details>
```
- [ ] **Cups don't kill, they only spawn them at the haul.  Other players can kill, but they don't die.  No one dies, they just have to crawl back.**

- [ ] **Make the cops respawn the player to jail position.**

- [ ] **cool article on how to move npcs [NPC Kit](https://developer.roblox.com/en-us/articles/npc-kit)**

- [ ] **I can set the max players per game.  Then, I can check player count, and show our destroy as many soldiers and such as I like depending on who is playing!?**

- [ ] **Things you can buy from the kids : Chips, dog, tic tax tie, jet pack (for if you crash). But if you land in an area...**

- [ ] **[The US Military Is Building Its Own Metaverse](https://www.wired.com/story/military-metaverse/)**

- [ ] **Red light green light game: The top light, vs the bottom light, could be a correlation. That's is the location of the object chasing in space could be that guys key. Then, when the cklor changes, they could correlate that. The location, as well as the color. It's as if all the information had to be diligently collected or no correlation will take place. No dara will be saved**
  
  <details>
  <summary>💬 Comments (2)</summary>
  
  **Comment 1:** Losing health is an automatic antecedent with every player
  
  **Comment 2:** I can save all the colors. Like,  on each anticident (which is not triggered unless match an antcedint from players list) but don't save colors on non antecedents.  Hmm
  
  </details>

- [ ] **There needs to be done constant increase,  either money or life points.   Probably life points.**

- [ ] **Need to add a decoding layer, that takes goToObj and translates roblox code.  So I need one decider pie world**

- [ ] **Shit!! This is AI I could take to get a feel for it, the code is free.  Hmmm.**
  > 📝 [Roblox_Sword_Fighting_AI/README.txt at master · idiomic/Roblox_Sword_Fighting_AI](https://github.com/idiomic/Roblox_Sword_Fighting_AI/blob/master/README.txt)
  
  <details>
  <summary>💬 Comments (1)</summary>
  
  **Comment 1:** OMG, he even adde the place file???  I want to check this out so bad now!!
https://devforum.roblox.com/t/how-to-make-ai/405865/24?page=2
  
  </details>

- [ ] **Roblox escape room**

- [ ] **Robl9x hunger games**

- [ ] **[Our Vision for the Roblox Economy - Roblox Blog](https://blog.roblox.com/2023/07/vision-roblox-economy/)**

- [ ] ****Cloud Park** = ·     This would be your portal to the internet ·     It would be your home page, that you share ·     You can add other peoples cloud’s to your park ·     See tweets and everything ·     See all your favorite web pages right there ·     Everything is just a few click away ·     Share anything and any file type**

- [ ] **CafeBots= a friend relationship manager**