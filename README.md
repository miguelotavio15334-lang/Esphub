-- MIIIGUEX V22 FIX - PEGA NA MÃO E VAI PRA BASE 100%
-- Fix do bug que não ia

local LP = game.Players.LocalPlayer
local Plots = workspace:WaitForChild("Plots")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_V22"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_V22"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 360, 0, 180)
main.Position = UDim2.new(0.5,-180,0.5,-90)
main.BackgroundColor3 = Color3.fromRGB(24,24,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,16)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,40)
title.Text = "MIIIGUEX V22 FIX - TESTE"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local bOn = Instance.new("TextButton", main)
bOn.Position = UDim2.new(0,15,0,50)
bOn.Size = UDim2.new(1,-30,0,60)
bOn.Text = "✈️ PEGOU NA MÃO -> BASE: ON"
bOn.BackgroundColor3 = Color3.fromRGB(0,170,80)
bOn.TextColor3 = Color3.new(1,1,1)
bOn.Font = Enum.Font.GothamBold
bOn.TextSize = 14
Instance.new("UICorner", bOn).CornerRadius = UDim.new(0,12)

local status = Instance.new("TextLabel", main)
status.Position = UDim2.new(0,15,0,120)
status.Size = UDim2.new(1,-30,0,40)
status.Text = "Status: esperando palhaço na mão..."
status.TextColor3 = Color3.fromRGB(200,200,200)
status.BackgroundTransparency = 1
status.Font = Enum.Font.GothamBold
status.TextSize = 11

local ativo = true

bOn.MouseButton1Click:Connect(function()
    ativo = not ativo
    bOn.Text = ativo and "✈️ PEGOU NA MÃO -> BASE: ON" or "✈️ PEGOU NA MÃO -> BASE: OFF"
    bOn.BackgroundColor3 = ativo and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30)
    status.Text = ativo and "Status: esperando..." or "Status: DESATIVADO"
end)

local function getMinhaBasePos()
    for _,p in pairs(Plots:GetChildren()) do
        if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then
            -- tenta achar o spawn da base
            if p:FindFirstChild("Delivery") then
                return p.Delivery.Position + Vector3.new(0,5,0)
            elseif p:FindFirstChild("Spawn") then
                return p.Spawn.Position + Vector3.new(0,5,0)
            else
                return p:GetPivot().Position + Vector3.new(0,5,0)
            end
        end
    end
    return nil
end

local jaFoi = false

-- LOOP QUE VERIFICA A CADA 0.1 SEGUNDO SE TA COM PALHAÇO NA MÃO
task.spawn(function()
    while true do
        task.wait(0.1)
        if ativo and not jaFoi then
            pcall(function()
                local char = LP.Character
                if not char then return end
                
                local tool = char:FindFirstChildWhichIsA("Tool")
                -- DETECTA SE TEM PALHAÇO NA MÃO (Tool com nome de Brainrot ou qualquer Tool)
                if tool then
                    jaFoi = true
                    status.Text = "Status: PEGOU "..tool.Name.."! INDO PRA BASE!"
                    
                    local basePos = getMinhaBasePos()
                    if basePos then
                        -- Noclip e VOA
                        for i=1,20 do
                            task.wait(0.02)
                            if char:FindFirstChild("HumanoidRootPart") then
                                char.HumanoidRootPart.CFrame = CFrame.new(basePos)
                                for _,v in pairs(char:GetDescendants()) do
                                    if v:IsA("BasePart") then v.CanCollide = false end
                                end
                            end
                        end
                        
                        task.wait(0.4)
                        status.Text = "Status: CHEGOU! KITANDO..."
                        
                        -- KITA
                        local t = char:FindFirstChildWhichIsA("Tool")
                        if t then
                            t.Parent = LP.Backpack
                        end
                        task.wait(0.2)
                        -- tenta soltar de outro jeito tambem
                        for _,r in pairs(game.ReplicatedStorage:GetDescendants()) do
                            if r:IsA("RemoteEvent") then
                                if r.Name:lower():find("drop") or r.Name:lower():find("place") then
                                    r:FireServer()
                                end
                            end
                        end
                        
                        status.Text = "Status: ✅ GUARDADO! Pode roubar dnv"
                        game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Palhaço guardado na base!", Duration=3})
                        
                        task.wait(2)
                        jaFoi = false -- reseta pra poder roubar dnv
                        status.Text = "Status: esperando palhaço na mão..."
                    else
                        status.Text = "Status: NÃO ACHEI TUA BASE!"
                        jaFoi = false
                    end
                end
            end)
        end
    end
end)

-- Se morrer reseta o jaFoi
LP.CharacterAdded:Connect(function()
    jaFoi = false
    status.Text = "Status: respawnou, esperando..."
end)

game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX V22", Text="V22 ATIVADO! Pega o palhaço na mão agora", Duration=3})
