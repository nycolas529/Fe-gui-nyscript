-- Script principal para criar a GUI no Roblox Studio
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Criação da tela GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

-- Criar o quadro de fundo da GUI
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Parent = screenGui

-- Função para permitir que a GUI seja movida
local dragging = false
local dragInput, dragStart, startPos

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragInput = input
        dragStart = input.Position
        startPos = frame.Position
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

frame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- Função de X-ray
local function activateXRay()
    local character = player.Character
    if character then
        local torso = character:FindFirstChild("HumanoidRootPart")
        if torso then
            torso.Transparency = 0.5  -- Torna o personagem semi-transparente
        end
    end
end

-- Função para teleportar para todos os jogadores
local function teleportToAllPlayers()
    local character = player.Character
    if character then
        for _, otherPlayer in ipairs(game.Players:GetPlayers()) do
            if otherPlayer ~= player then
                local otherCharacter = otherPlayer.Character
                if otherCharacter and otherCharacter:FindFirstChild("HumanoidRootPart") then
                    character:SetPrimaryPartCFrame(otherCharacter.HumanoidRootPart.CFrame)
                end
            end
        end
    end
end

-- Função para ativar o voo
local function startFlying()
    local character = player.Character
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.PlatformStand = true
        humanoid:ChangeState(Enum.HumanoidStateType.Physics)
        
        -- Cria um BodyVelocity para permitir o voo
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(40000, 40000, 40000)
        bodyVelocity.Velocity = Vector3.new(0, 50, 0) -- A velocidade para começar a voar
        bodyVelocity.Parent = character:FindFirstChild("HumanoidRootPart")
    end
end

-- Botão 1 - X-ray
local button1 = Instance.new("TextButton")
button1.Size = UDim2.new(0, 280, 0, 50)
button1.Position = UDim2.new(0, 10, 0, 10)
button1.Text = "Ativar X-ray"
button1.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
button1.Parent = frame
button1.MouseButton1Click:Connect(activateXRay)

-- Botão 2 - TP a todos os jogadores
local button2 = Instance.new("TextButton")
button2.Size = UDim2.new(0, 280, 0, 50)
button2.Position = UDim2.new(0, 10, 0, 70)
button2.Text = "Teletransportar para todos"
button2.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
button2.Parent = frame
button2.MouseButton1Click:Connect(teleportToAllPlayers)

-- Botão 3 - Voar
local button3 = Instance.new("TextButton")
button3.Size = UDim2.new(0, 280, 0, 50)
button3.Position = UDim2.new(0, 10, 0, 130)
button3.Text = "Ativar Voo"
button3.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
button3.Parent = frame
button3.MouseButton1Click:Connect(startFlying)

