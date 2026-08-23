-- Roba un payaso — Script Creado por MIIIGUEX GOD
-- Auto Roubar + Auto Vender + Auto Server Hop

local LP = game.Players.LocalPlayer
local PlayerGui = LP:WaitForChild("PlayerGui")
local TeleportService = game:GetService("TeleportService")
local PlaceId = game.PlaceId

pcall(function() PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

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

local function Label(txt,y)
    local l = Instance.new("TextLabel", main)
    l.Position = UDim2.new(0.05,0,0,y)
    l.Size = UDim2.new(0.9,0,0,25)
    l.Text = txt
    l.TextColor3 = Color3.new(1,1,1)
    l.BackgroundTransparency = 1
    l.Font = Enum.Font.GothamBold
    l.TextSize = 14
    l.TextXAlignment = Enum.TextXAlignment.Left
    return l
end

local function Btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Size = UDim2.new(0.9,0,0,42)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(35,40,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.Gotham
    b.TextSize = 14
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,9)
    return b
end

local t1 = Instance.new("TextLabel", main)
t1.Size = UDim2.new(1,0,0,40)
t1.Text = "Roba un payaso — Script Creado por MIIIGUEX"
t1.TextColor3 = Color3.new(1,1,1)
t1.BackgroundTransparency = 1
t1.Font = Enum.Font.GothamBold
t1.TextSize = 13
t1.TextScaled = true

local t2 = Instance.new("TextLabel", main)
t2.Position = UDim2.new(0.05,0,0,40)
t2.Size = UDim2.new(0.9,0,0,15)
t2.Text = "script se irá actualizando"
t2.TextColor3 = Color3.fromRGB(150,150,150)
t2.BackgroundTransparency = 1
t2.TextSize = 11

Label("Funciones", 65)
local btnSave = Btn("Guardar Posición", 95)
local btnTP = Btn("TP Guardado", 145)
local btnNoclip = Btn("NoClip: OFF", 195)

Label("Auto Farm MIIIGUEX", 250)
local btnAutoAll = Btn("🤡 AUTO ROUBAR TODOS: OFF", 280, Color3.fromRGB(120,30,30))
local btnAutoSell = Btn("💰 AUTO VENDER: OFF", 330, Color3.fromRGB(120,30,30))
local btnAutoHop = Btn("🔄 AUTO TROCAR SERVER: OFF", 380, Color3.fromRGB(120,30,30))
local btnClose = Btn("Cerrar", 430, Color3.fromRGB(50,50,50))

-- LÓGICA
local savedPos = nil
local noclip = false
local autoSteal, autoSell, autoHop = false, false, false

btnSave.MouseButton1Click:Connect(function() savedPos = LP.Character.HumanoidRootPart.CFrame game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX",Text="Posição salva!",Duration=1}) end)
btnTP.MouseButton1Click:Connect(function() if savedPos then LP.Character.HumanoidRootPart.CFrame = savedPos end end)
btnNoclip.MouseButton1Click:Connect(function() noclip = not noclip btnNoclip.Text = noclip and "NoClip: ON" or "NoClip: OFF" end)

btnAutoAll.MouseButton1Click:Connect(function() autoSteal = not autoSteal btnAutoAll.Text = autoSteal and "🤡 AUTO ROUBAR TODOS: ON" or "🤡 AUTO ROUBAR TODOS: OFF" btnAutoAll.BackgroundColor3 = autoSteal and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnAutoSell.MouseButton1Click:Connect(function() autoSell = not autoSell btnAutoSell.Text = autoSell and "💰 AUTO VENDER: ON" or "💰 AUTO VENDER: OFF" btnAutoSell.BackgroundColor3 = autoSell and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnAutoHop.MouseButton1Click:Connect(function() autoHop = not autoHop btnAutoHop.Text = autoHop and "🔄 AUTO TROCAR SERVER: ON" or "🔄 AUTO TROCAR SERVER: OFF" btnAutoHop.BackgroundColor3 = autoHop and Color3.fromRGB(0,170,80) or Color3.fromRGB(120,30,30) end)
btnClose.MouseButton1Click:Connect(function() gui:Destroy() end)

-- NoClip loop
game:GetService("RunService").Stepped:Connect(function()
    if noclip or autoSteal then
        for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
    end
end)

-- 1. AUTO ROUBAR TODOS (não filtra mais por valor)
task.spawn(function()
    while true do
        task.wait(0.3)
        if autoSteal then
            pcall(function()
                local segurando = LP.Character:FindFirstChildWhichIsA("Tool") ~= nil
                if segurando then
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                            LP.Character.HumanoidRootPart.CFrame = plot:GetPivot() + Vector3.new(0,8,0)
                        end
                    end
                else
                    local maisPerto, dist = nil, math.huge
                    for _,plot in pairs(workspace.Plots:GetChildren()) do
                        if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                            for _,clown in pairs(plot:GetChildren()) do
                                if clown:FindFirstChild("PrimaryPart") and clown:FindFirstChild("PricePerSecond") then
                                    local d = (LP.Character.HumanoidRootPart.Position - clown.PrimaryPart.Position).Magnitude
                                    if d < dist then dist = d maisPerto = clown end
                                end
                            end
                        end
                    end
                    if maisPerto then
                        LP.Character.HumanoidRootPart.CFrame = maisPerto.PrimaryPart.CFrame + Vector3.new(0,2,0)
                        task.wait(0.2)
                        for _,p in pairs(maisPerto:GetDescendants()) do
                            if p:IsA("ProximityPrompt") then p.HoldDuration = 0 p.MaxActivationDistance = 100 fireproximityprompt(p) end
                        end
                    else
                        -- se não tem mais nada, troca de server
                        if autoHop then
                            TeleportService:Teleport(PlaceId, LP)
                        end
                    end
                end
            end)
        end
    end
end)

-- 2. AUTO VENDER (procura botão de vender da tua base)
task.spawn(function()
    while true do
        task.wait(1)
        if autoSell then
            pcall(function()
                for _,plot in pairs(workspace.Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then
                        for _,v in pairs(plot:GetDescendants()) do
                            if v.Name:lower():find("sell") and v:IsA("ProximityPrompt") then
                                fireproximityprompt(v)
                            end
                            if v.Name:lower():find("sell") and v:IsA("TouchTransmitter") then
                                firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 0)
                                firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 1)
                            end
                        end
                    end
                end
            end)
        end
    end
end)

game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX",Text="Menu GOD carregado!",Duration=3})
