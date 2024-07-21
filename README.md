local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()
local Mobs_Table = {}
for _, v in ipairs(workspace.NPC.Fight:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not table.find(Mobs_Table, v.Name) then
        table.insert(Mobs_Table, v.Name)
        table.sort(Mobs_Table)
    end
end
local PlayerTP1
local TweenService = game:GetService("TweenService")
local X = Material.Load({
	Title = "🗡️ Easy Piece 🎉",
	Style = 3,
	SizeX = 320,
	SizeY = 320,
	Theme = "Dark",
	ColorOverrides = {
		MainFrame = Color3.fromRGB(0,9,55)
	}
})

local Y = X.New({
	Title = "Main"
})
local ii = Y.Dropdown({
	Text = "Select Mobs!",
	Callback = function(t)
        PlayerTP1 = t
	end,
	Options = Mobs_Table
})
Y.Dropdown({
	Text = "Select Weapon!",
	Callback = function(t)
	Weapons = t
	end,
	Options = {
			"Combat",
			"Dark Blade",

	},
})
Y.Button(
    {
        Text = "Refresh|Mobs",
        Callback = function()
            table.clear(Mobs_Table)
            for i, v in pairs(workspace.NPC.Fight:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(Mobs_Table, tostring(v)) then
                        table.insert(Mobs_Table, tostring(v))
                        ii:SetOptions(Mobs_Table)
                    end
                end
            end
        end
    }
)
Y.Toggle({
Text = "Auto Farm/Mobs",
Callback = function(Value)
_G.Attack = Value
while _G.Attack do
task.wait(1)
pcall(function()
game.workspace.Gravity = 0
hrp = game.Players.LocalPlayer.Character.PrimaryPart
for i,v in pairs(workspace.NPC.Fight:GetDescendants()) do
if v.Name == PlayerTP1 then
if v.Humanoid.Health > 0 then
repeat
local toolName = Weapons -- Replace "YourToolNameHere" with the name of your tool 
local LocalPlayer = game:GetService("Players").LocalPlayer
for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
if v:IsA('Tool') and v.Name == toolName then
    v.Parent = LocalPlayer.Character
    break -- Stop the loop after picking up the tool
end
end
LocalPlayer.Character:FindFirstChild(toolName):Activate()
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,4)
wait()  -- Wait a short time before checking again
until not _G.Attack or v.Humanoid.Health <= 0
end
end
end
end)
end
end,
Enabled = false
})
