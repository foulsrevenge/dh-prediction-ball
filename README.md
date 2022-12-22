# dh-prediction-ball
short script with no ui because im bad at coding



--███████╗██████╗░░█████╗░--   change toggle key on line 11
--██╔════╝██╔══██╗██╔══██╗--
--█████╗░░██████╔╝███████║--   change dot size on line 36
--██╔══╝░░██╔══██╗██╔══██║--
--███████╗██║░░██║██║░░██║--   change dot color on line 83
--╚══════╝╚═╝░░╚═╝╚═╝░░╚═╝--
     --enjoy you skid-- 
getgenv().Settings = {
	rewrittenmain = {
		Enabled = true,
		Key = "q",     --CHANGE TOGGLE KEY HERE!!--
		DOT = true,
		FOV = math.huge,
	}
}
local Settings = getgenv().Settings
getgenv().SelectedPart = "HumanoidRootPart"
getgenv().Prediction = true
getgenv().PredictionValue = 0.146
local AnchorCount = 0
local MaxAnchor = 50
local CC = game:GetService"Workspace".CurrentCamera
local Plr;
local enabled = false
local accomidationfactor = 0.139
local mouse = game.Players.LocalPlayer:GetMouse()
local placemarker = Instance.new("Part", game.Workspace)
function makemarker(Parent, Adornee, Color, Size, Size2)
	local e = Instance.new("BillboardGui", Parent)
	e.Name = "PP"
	e.Adornee = Adornee
	e.Size = UDim2.new(Size, Size2, Size, Size2)
	e.AlwaysOnTop = Settings.rewrittenmain.DOT
	local a = Instance.new("Frame", e)
	if Settings.rewrittenmain.DOT == true then
		a.Size = UDim2.new(2, 2, 2,2)         --CHANGE DOT SIZE HERE!!--
	else
		a.Size = UDim2.new(0, 0, 0, 0)
	end
	if Settings.rewrittenmain.DOT == true then
		a.Transparency = 0
		a.BackgroundTransparency = 0
	else
		a.Transparency = 1
		a.BackgroundTransparency = 1
	end
	a.BackgroundColor3 = Color
	local g = Instance.new("UICorner", a)
	if Settings.rewrittenmain.DOT == false then
		g.CornerRadius = UDim.new(0, 0)
	else
		g.CornerRadius = UDim.new(1, 1) 
	end
	return(e)
end
local data = game.Players:GetPlayers()
function noob(player)
	local character
	repeat wait() until player.Character
	local handler = makemarker(guimain, player.Character:WaitForChild(SelectedPart), Color3.fromRGB(255,255,0), 0.4, 4)
	handler.Name = player.Name
	player.CharacterAdded:connect(function(Char) handler.Adornee = Char:WaitForChild(SelectedPart) end)
	spawn(function()
		while wait() do
			if player.Character then
			end
		end
	end)
end
game.Players.PlayerAdded:connect(function(Player)
	noob(Player)
end)
spawn(function()
	placemarker.Anchored = true
	placemarker.CanCollide = false
	if Settings.rewrittenmain.DOT == true then
		placemarker.Size = Vector3.new(0, 0, 0)
	else
		placemarker.Size = Vector3.new(0, 0, 0)
	end
	placemarker.Transparency = 0
	if Settings.rewrittenmain.DOT then
		makemarker(placemarker, placemarker, Color3.fromRGB(255,255,0), 0.40, 0)  --CHANGE DOT COLOR HERE!!--
	end
end)
game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(k)
	if k == Settings.rewrittenmain.Key and Settings.rewrittenmain.Enabled then
		if enabled == true then
			enabled = false
		else
			Plr = getClosestPlayerToCursor()
			enabled = true
			if Settings.rewrittenmain.NOTIF == true then
			end
		end
	end
end)
function getClosestPlayerToCursor()
	local closestPlayer
	local shortestDistance = Settings.rewrittenmain.FOV

	for i, v in pairs(game.Players:GetPlayers()) do
		if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild("HumanoidRootPart") then
			local pos = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
			local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).magnitude
			if magnitude < shortestDistance then
				closestPlayer = v
				shortestDistance = magnitude
			end
		end
	end
	return closestPlayer
end
local pingvalue = nil;
local split = nil;
local ping = nil;
game:GetService"RunService".Stepped:connect(function()
	if enabled and Plr.Character ~= nil and Plr.Character:FindFirstChild("HumanoidRootPart") then
		placemarker.CFrame = CFrame.new(Plr.Character.HumanoidRootPart.Position+(Plr.Character.HumanoidRootPart.Velocity*accomidationfactor))
	else
		placemarker.CFrame = CFrame.new(0, 9999, 0)
	end
	if Settings.rewrittenmain.AUTOPRED == true then
		pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
		split = string.split(pingvalue,'(')
		ping = tonumber(split[1])
		if ping < 130 then
			PredictionValue = 0.151
		elseif ping < 125 then
			PredictionValue = 0.149
		elseif ping < 110 then
			PredictionValue = 0.146
		elseif ping < 105 then
			PredictionValue = 0.138
		elseif ping < 90 then
			PredictionValue = 0.136
		elseif ping < 80 then
			PredictionValue = 0.134
		elseif ping < 70 then
			PredictionValue = 0.131
		elseif ping < 60 then
			PredictionValue = 0.1229
		elseif ping < 50 then
			PredictionValue = 0.1225
		elseif ping < 40 then
			PredictionValue = 0.1256
		end
	end
end)
