local player = game.Players.LocalPlayer
local replicated = game:GetService("ReplicatedStorage")
local runService = game:GetService("RunService")

local autoFarm = false
local gui = Instance.new("ScreenGui", game.CoreGui)
local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0.1, 0, 0.1, 0)
button.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
button.Text = "Auto Farm (OFF)"
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 18

-- Função para detectar o sea atual
local function detectarSea()
	local lugar = game.PlaceId
	if lugar == 2753915549 then
		return "First Sea"
	elseif lugar == 4442272183 then
		return "Second Sea"
	elseif lugar == 7449423635 then
		return "Third Sea"
	else
		return "Desconhecido"
	end
end

-- Função para encontrar NPC inimigo mais próximo
local function encontrarNPC()
	for _, v in pairs(workspace.Enemies:GetChildren()) do
		if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
			return v
		end
	end
	return nil
end
local function iniciarAutoFarm()
	while autoFarm do
		local npc = encontrarNPC()
		if npc then
			local char = player.Character
			if char and char:FindFirstChild("HumanoidRootPart") then
				char.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
				replicated.Remotes.CommF_:InvokeServer("Attack")
			end
		end
		wait(0.5)
	end
end

button.MouseButton1Click:Connect(function()
	autoFarm = not autoFarm
	button.Text = autoFarm and "Auto Farm (ON)" or "Auto Farm (OFF)"
	if autoFarm then
		task.spawn(iniciarAutoFarm)
	end
end)


local seaLabel = Instance.new("TextLabel", gui)
seaLabel.Size = UDim2.new(0, 200, 0, 30)
seaLabel.Position = UDim2.new(0.1, 0, 0.05, 0)
seaLabel.BackgroundTransparency = 1
seaLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
seaLabel.Font = Enum.Font.SourceSansBold
seaLabel.TextSize = 18
seaLabel.Text = "Sea atual: " .. detectarSea()
local plr = game.Players.LocalPlayer
local replicated = game:GetService("ReplicatedStorage")
local runService = game:GetService("RunService")
local autoFarm = false


local gui = Instance.new("ScreenGui", game.CoreGui)
local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0.1, 0, 0.1, 0)
button.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
button.Text = "Auto Farm (OFF)"
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 18
local questData = {
	
	{min = 10, max = 50, npc = "Bandit [Lv. 5]", quest = "BanditQuest1", part = "Bandit", pos = CFrame.new(1060, 16, 1548)},
	{min = 50, max = 120, npc = "Pirate [Lv. 35]", quest = "BuggyQuest1", part = "Pirate", pos = CFrame.new(-1145, 14, 3828)},
	
	{min = 700, max = 800, npc = "Raider [Lv. 700]", quest = "Area1Quest", part = "Raider", pos = CFrame.new(-495, 400, 9168)},
	
	{min = 1500, max = 1575, npc = "Pirate Millionaire [Lv. 1500]", quest = "PiratePortQuest", part = "Pirate Millionaire", pos = CFrame.new(-287, 44, 5589)},
}

local function detectarSea()
	local placeId = game.PlaceId
	if placeId == 2753915549 then
		return "First Sea"
	elseif placeId == 4442272183 then
		return "Second Sea"
	elseif placeId == 7449423635 then
		return "Third Sea"
	else
		return "Desconhecido"
	end
end


local function getQuest()
	local level = plr.Data.Level.Value
	for _, q in pairs(questData) do
		if level >= q.min and level <= q.max then
			return q
		end
	end
	return nil
end


local function pegarMissao(quest)
	if quest then
		plr.Character.HumanoidRootPart.CFrame = quest.pos
		wait(1)
		replicated.Remotes.CommF_:InvokeServer("QuestAccepted", quest.quest, 1)
	end
end

local function encontrarNPC(nome)
	for _, v in pairs(workspace.Enemies:GetChildren()) do
		if v.Name == nome and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
			return v
		end
	end
	return nil
end


local function iniciarAutoFarm()
	while autoFarm do
		local quest = getQuest()
		if quest then
			if not plr.PlayerGui:FindFirstChild("QuestGUI") then
				pegarMissao(quest)
			end
			local inimigo = encontrarNPC(quest.npc)
			if inimigo then
				plr.Character.HumanoidRootPart.CFrame = inimigo.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
				replicated.Remotes.CommF_:InvokeServer("Attack")
			end
		end
		wait(0.5)
	end
end

button.MouseButton1Click:Connect(function()
	autoFarm = not autoFarm
	button.Text = autoFarm and "Auto Farm (ON)" or "Auto Farm (OFF)"
	if autoFarm then
		task.spawn(iniciarAutoFarm)
	
end)

local gui = Instance.new("ScreenGui", game.CoreGui)
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 250, 0, 240)
frame.Position = UDim2.new(0.02, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true

local titulo = Instance.new("japa hub", frame)
titulo.Size = UDim2.new(0, 250, 0, 30)
titulo.Position = UDim2.new(0, 0, 0, -30)
titulo.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.Text = "japa hub"
titulo.Font = Enum.Font.SourceSansBold
titulo.TextSize = 20

-- Função para criar botões
local function criarBotao(nome, ordem, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(0, 230, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, ordem * 45)
	btn.Text = nome
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(50,50,50)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 18
	btn.MouseButton1Click:Connect(callback)
	return btn
end


local autoFarm = false
local usarEspada = true
local autoHaki = true
local autoBoss = true


local farmBtn = criarBotao("Auto Farm (OFF)", 0, function()
	autoFarm = not autoFarm
	farmBtn.Text = autoFarm and "Auto Farm (ON)" or "Auto Farm (OFF)"
	if autoFarm then task.spawn(autoFarmar) end
end)

local modoBtn = criarBotao("Modo: Espada", 1, function()
	usarEspada = not usarEspada
	modoBtn.Text = usarEspada and "Modo: Espada" or "Modo: Fruta"
end)

local hakiBtn = criarBotao("Auto Haki (ON)", 2, function()
	autoHaki = not autoHaki
	hakiBtn.Text = autoHaki and "Auto Haki (ON)" or "Auto Haki (OFF)"
end)

local bossBtn = criarBotao("Farm Bosses (ON)", 3, function()
	autoBoss = not autoBoss
	bossBtn.Text = autoBoss and "Farm Bosses (ON)" or "Farm Bosses (OFF)"
end)
