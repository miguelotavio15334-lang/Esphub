-- FIX ROUBAR E VENDER - MIIIGUEX V12
-- Apaga o hub antigo e cola esse

local LP = game.Players.LocalPlayer
local PlayerGui = LP:WaitForChild("PlayerGui")
pcall(function() PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

-- MESMO MENU QUE TU JÁ TEM, SÓ QUE COM SISTEMA NOVO
local gui = Instance.new("ScreenGui", PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 320, 0, 480)
main.Position = UDim2.new(0.5,-160,0.5,-240)
main.BackgroundColor3 = Color3.fromRGB(22,22,27)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)

local function Btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Size = UDim2.new(0.9,0,0,42)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(35,40,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 13
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,9)
    return b
end

local t1 = Instance.new("TextLabel", main)
t1.Size = UDim2.new(1,0,0,50)
t1.Text = "Roba un payaso — Script Creado por MIIIGUEX\nFIX V12"
t1.TextColor3 = Color3.new(1,1,1)
t1.BackgroundTransparency = 1
t1.Font = Enum.Font.GothamBold
t1.TextSize = 13

local btnNoclip = Btn("NoClip: OFF", 60)
local btnAutoAll = Btn("🤡 AUTO ROUBAR TODOS: OFF", 110, Color3.fromRGB(120,30,30))
local btnAutoSell = Btn("💰 AUTO VENDER: OFF", 160, Color3.fromRGB(120,30,30))
local btnAutoHop = Btn("🔄 AUTO TROCAR SERVER: OFF", 210, Color3.fromRGB(120,30,30))
local btnClose = Btn("Cerrar", 260, Color3.fromRGB(50,50,50))

local noclip, autoSteal, autoSell, autoHop = false, false, false, false

btnNoclip.MouseButton1Click:Connect(function() noclip = not noclip btnNoclip.Text = noclip and "NoClip: ON" or "NoClip: OFF" btnNoclip.BackgroundColor3 = noclip and Color3.fromRGB(0,170,80) or Color3.fromRGB(35,40,55) end)
btnAutoAll.MouseButton1Click:Connect(function() autoSteal = not autoSteal btnAutoAll.Text = autoSteal and "🤡 AUTO ROUBAR TODOS: ON" or "🤡 AUTO ROUBAR TODOS: OFF" btnAutoAll.BackgroundColor3 = autoSteal and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnAutoSell.MouseButton1Click:Connect(function() autoSell = not autoSell btnAutoSell.Text = autoSell and "💰 AUTO VENDER: ON" or "💰 AUTO VENDER: OFF" btnAutoSell.BackgroundColor3 = autoSell and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnAutoHop.MouseButton1Click:Connect(function() autoHop = not autoHop btnAutoHop.Text = autoHop and "🔄 AUTO TROCAR SERVER: ON" or "🔄 AUTO TROCAR SERVER: OFF" btnAutoHop.BackgroundColor3 = autoHop and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnClose.MouseButton1Click:Connect(function() gui:Destroy() end)

game:GetService("RunService").Stepped:Connect(function()
    if noclip or autoSteal then
        for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
    end
end)

-- NOVO SISTEMA DE ROUBO - TESTADO HOJE
task.spawn(function()
    while true do
        task.wait(0.2)
        if autoSteal then
            pcall(function()
                -- se tá segurando, leva pra base
                if LP.Character:FindFirstChildWhichIsA("Tool") or LP.Character:FindFirstChild("Clown") then
                    for _,plot in pairs(workspace:FindFirstChild("Plots"):GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                            -- vai no centro da base
                            LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() * CFrame.new(0,5,0)
                        end
                    end
                else
                    -- procura palhaço
                    local melhor = nil
                    local distMenor = math.huge
                    for _,plot in pairs(workspace:FindFirstChild("Plots"):GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                            for _,obj in pairs(plot:GetChildren()) do
                                if obj:IsA("Model") and obj.PrimaryPart then
                                    -- pega QUALQUER modelo que seja palhaço
                                    local d = (LP.Character.HumanoidRootPart.Position - obj.PrimaryPart.Position).Magnitude
                                    if d < distMenor then
                                        distMenor = d
                                        melhor = obj
                                    end
                                end
                            end
                        end
                    end
                    if melhor then
                        LP.Character.HumanoidRootPart.CFrame = melhor.PrimaryPart.CFrame + Vector3.new(0,2,2)
                        task.wait(0.15)
                        -- TENTA TODOS OS JEITOS DE ROUBAR
                        for _,v in pairs(melhor:GetDescendants()) do
                            if v:IsA("ProximityPrompt") then
                                v.HoldDuration = 0
                                v.MaxActivationDistance = 100
                                fireproximityprompt(v)
                            end
                            if v:IsA("ClickDetector") then
                                fireclickdetector(v)
                            end
                        end
                        -- tenta remote
                        local rs = game:GetService("ReplicatedStorage")
                        for _,rem in pairs(rs:GetDescendants()) do
                            if rem:IsA("RemoteEvent") and rem.Name:lower():find("steal") or rem.Name:lower():find("rob") or rem.Name:lower():find("buy") then
                                rem:FireServer(melhor)
                            end
                        end
                    else
                        if autoHop then
                            game:GetService("TeleportService"):Teleport(game.PlaceId, LP)
                        end
                    end
                end
            end)
        end
    end
end)

-- AUTO VENDER FIX
task.spawn(function()
    while true do
        task.wait(0.5)
        if autoSell then
            pcall(function()
                for _,plot in pairs(workspace:FindFirstChild("Plots"):GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                        for _,v in pairs(plot:GetDescendants()) do
                            if v:IsA("ProximityPrompt") and (v.Name:lower():find("sell") or v.ObjectText:lower():find("sell") or v.ActionText:lower():find("sell")) then
                                LP.Character.HumanoidRootPart.CFrame = v.Parent.CFrame or v.Parent:GetPivot()
                                task.wait(0.2)
                                fireproximityprompt(v)
                            end
                        end
                    end
                end
            end)
        end
    end
end)
