-- ✅ GUI Simples com botão de ativar
local autofarm = false

-- Criar interface
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "AutoFarmDelta"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

local uicorner = Instance.new("UICorner", frame)

local button = Instance.new("TextButton", frame)
button.Size = UDim2.new(0, 180, 0, 50)
button.Position = UDim2.new(0, 10, 0, 25)
button.Text = "Ativar Autofarm"
button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
button.TextColor3 = Color3.fromRGB(255, 255, 255)

-- 🔄 Função do autofarm
local function getClosestEnemy()
    local closest, dist = nil, math.huge
    for _, v in pairs(workspace:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.HumanoidRootPart.Position).magnitude
            if mag < dist then
                closest = v
                dist = mag
            end
        end
    end
    return closest
end

-- 🟢 Loop do autofarm
button.MouseButton1Click:Connect(function()
    autofarm = not autofarm
    button.Text = autofarm and "Desativar Autofarm" or "Ativar Autofarm"
    button.BackgroundColor3 = autofarm and Color3.fromRGB(170, 0, 0) or Color3.fromRGB(0, 170, 0)

    while autofarm do
        local enemy = getClosestEnemy()
        if enemy then
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                char.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
                -- ⚔️ Simular ataque (pressiona clique esquerdo)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, 0)
                wait(0.1)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, 0)
            end
        end
        wait(0.5)
    end
end)
