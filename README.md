--// Bluudud Local Script //--
-- Estilo: C00lkidd GUI azul neon, móvel, com scroll e botão abrir/fechar
-- Criado por GPT-5 com base no personagem Bluudud Forsaken

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")
local root = char:WaitForChild("HumanoidRootPart")

--// GUI Principal
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "BluududGui"
screenGui.ResetOnSpawn = false

--// Frame Principal
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.Size = UDim2.new(0, 330, 0, 420)
mainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false -- começa fechado

Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 16)

local stroke = Instance.new("UIStroke", mainFrame)
stroke.Color = Color3.fromRGB(0, 120, 255)
stroke.Thickness = 2

local gradient = Instance.new("UIGradient", mainFrame)
gradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 50, 150)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 150, 255))
})
gradient.Rotation = 45

--// Título
local title = Instance.new("TextLabel", mainFrame)
title.Text = "BLUUDUD POWERS"
title.Font = Enum.Font.Code
title.TextColor3 = Color3.fromRGB(0, 170, 255)
title.TextScaled = true
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, 0, 0, 40)

--// ScrollFrame
local scroll = Instance.new("ScrollingFrame", mainFrame)
scroll.Position = UDim2.new(0, 10, 0, 45)
scroll.Size = UDim2.new(1, -20, 1, -55)
scroll.CanvasSize = UDim2.new(0, 0, 2, 0)
scroll.ScrollBarThickness = 8
scroll.ScrollBarImageColor3 = Color3.fromRGB(0, 120, 255)
scroll.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 8)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

--// Botão de abrir/fechar
local toggleButton = Instance.new("TextButton", screenGui)
toggleButton.Size = UDim2.new(0, 120, 0, 35)
toggleButton.Position = UDim2.new(0, 10, 1, -45)
toggleButton.AnchorPoint = Vector2.new(0, 1)
toggleButton.BackgroundColor3 = Color3.fromRGB(10, 10, 40)
toggleButton.Text = "☰ BLUUDUD"
toggleButton.TextColor3 = Color3.fromRGB(0, 150, 255)
toggleButton.Font = Enum.Font.Code
toggleButton.TextScaled = true
Instance.new("UICorner", toggleButton).CornerRadius = UDim.new(0, 10)

local btnStroke = Instance.new("UIStroke", toggleButton)
btnStroke.Color = Color3.fromRGB(0, 120, 255)
btnStroke.Thickness = 1.5

local open = false
toggleButton.MouseButton1Click:Connect(function()
	open = not open
	mainFrame.Visible = open
end)

--// Torna a interface móvel
local UIS = game:GetService("UserInputService")
local dragging = false
local dragStart, startPos

mainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = mainFrame.Position
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

--// Funções dos poderes
local function criarParticulas(cor)
	local emitter = Instance.new("ParticleEmitter")
	emitter.Texture = "rbxassetid://241594314"
	emitter.Color = ColorSequence.new(cor)
	emitter.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.6), NumberSequenceKeypoint.new(1, 1.2)})
	emitter.Lifetime = NumberRange.new(0.5, 1)
	emitter.Rate = 150
	emitter.Speed = NumberRange.new(2, 5)
	emitter.Parent = root
	task.delay(3, function() emitter.Enabled = false; task.wait(2); emitter:Destroy() end)
end

local function auraAzul() criarParticulas(Color3.fromRGB(0, 150, 255)) end

local function fogoAzul()
	for i = 1, 3 do
		local fogo = Instance.new("Fire", root)
		fogo.Size = 15
		fogo.Heat = 20
		fogo.Color = Color3.fromRGB(0, 120, 255)
		fogo.SecondaryColor = Color3.fromRGB(0, 0, 255)
		task.delay(4, function() fogo:Destroy() end)
	end
end

local function formaAzulNegra()
	criarParticulas(Color3.fromRGB(0, 0, 255))
	game.Lighting.ColorShift_Bottom = Color3.fromRGB(0, 0, 50)
	game.Lighting.ColorShift_Top = Color3.fromRGB(0, 0, 100)
end

local function explosao()
	local blast = Instance.new("Explosion")
	blast.BlastRadius = 10
	blast.BlastPressure = 0
	blast.Position = root.Position
	blast.Parent = workspace
	criarParticulas(Color3.fromRGB(0, 100, 255))
end

local function teleporte()
	local look = root.CFrame.LookVector
	root.CFrame = root.CFrame + look * 20
	criarParticulas(Color3.fromRGB(0, 150, 255))
end

local function velocidade()
	hum.WalkSpeed = 50
	task.delay(5, function() hum.WalkSpeed = 16 end)
end

local function raio()
	local beamPart = Instance.new("Part", workspace)
	beamPart.Anchored = true
	beamPart.CanCollide = false
	beamPart.CFrame = root.CFrame * CFrame.new(0, 0, -5)
	local beam = Instance.new("Beam", beamPart)
	beam.Color = ColorSequence.new(Color3.fromRGB(0, 120, 255))
	beam.Width0, beam.Width1 = 1, 1
	task.delay(0.4, function() beamPart:Destroy() end)
end

local function dominioAzul()
	local sphere = Instance.new("Part", workspace)
	sphere.Shape = Enum.PartType.Ball
	sphere.Color = Color3.fromRGB(0, 0, 150)
	sphere.Anchored = true
	sphere.Size = Vector3.new(20, 20, 20)
	sphere.Material = Enum.Material.Neon
	sphere.Position = root.Position
	task.delay(5, function() sphere:Destroy() end)
end

local function glitch()
	game.Lighting.Blur = Instance.new("BlurEffect", game.Lighting)
	game.Lighting.Blur.Size = 20
	task.delay(2, function() game.Lighting.Blur:Destroy() end)
end

local function resetar()
	for _, v in pairs(root:GetChildren()) do
		if v:IsA("Fire") or v:IsA("ParticleEmitter") then
			v:Destroy()
		end
	end
	hum.WalkSpeed = 16
	game.Lighting.ColorShift_Top = Color3.fromRGB(0, 0, 0)
	game.Lighting.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
end

--// Lista de poderes
local botoes = {
	{"Aura Azul", auraAzul},
	{"Fogo Azul", fogoAzul},
	{"Forma Azul-Negra", formaAzulNegra},
	{"Explosão Dimensional", explosao},
	{"Teleporte", teleporte},
	{"Velocidade Bluudud", velocidade},
	{"Choque de Energia", raio},
	{"Domínio Azul", dominioAzul},
	{"Corrupt Mode", glitch},
	{"Reset Visual", resetar}
}

for _, data in ipairs(botoes) do
	local b = Instance.new("TextButton", scroll)
	b.Size = UDim2.new(0.9, 0, 0, 35)
	b.BackgroundColor3 = Color3.fromRGB(10, 10, 40)
	b.TextColor3 = Color3.fromRGB(0, 150, 255)
	b.Font = Enum.Font.Code
	b.TextScaled = true
	b.Text = data[1]
	b.AutoButtonColor = true
	Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
	b.MouseButton1Click:Connect(data[2])
end
