-- Roba un payaso — Script Creado por MIIIGUEX V15
-- Igual da tua foto + Auto Kitar K + Hop 100K/500K/1M

local LP = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local TS = game:GetService("TeleportService")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 380, 0, 620)
main.Position = UDim2.new(0.5, -190, 0.5, -310)
main.BackgroundColor3 = Color3.fromRGB(24,24,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 18)

-- Titulo
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, -20, 0, 60)
title.Position = UDim2.new(0,10,0,0)
title.Text = "Roba un payaso —\nScript Creado por MIIIGUEX"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.TextXAlignment = Enum.TextXAlignment.Left

-- container Acciones
local function criarLabel(txt,y)
    local l = Instance.new("TextLabel", main)
    l.Position = UDim2.new(0,15,0,y)
    l.Size = UDim2.new(1,-30,0,25)
    l.Text = txt
    l.TextColor3 = Color3.fromRGB(220,220,220)
    l.BackgroundTransparency = 1
    l.Font = Enum.Font.GothamBold
    l.TextSize = 15
    l.TextXAlignment = Enum.TextXAlignment.Left
    return l
end

local function criarBtn(txt,y)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0,15,0,y)
    b.Size = UDim2.new(1,-30,0,52)
    b.Text = txt
    b.BackgroundColor3 = Color3.fromRGB(45,47,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 15
    b.AutoButtonColor = false
    local c = Instance.new("UICorner", b)
    c.CornerRadius = UDim.new(0,12)
    local s = Instance.new("UIStroke", b)
    s.Color = Color3.fromRGB(70,70,80)
    s.Thickness = 1
    return b
end

criarLabel("Acciones", 65)
local bSave = criarBtn("Guardar Posición", 95)
local bTP = criarBtn("TP Guardado", 155)
local bNo = criarBtn("NoClip: OFF", 215)
local bKitar = criarBtn("Auto Kitar [K]: OFF", 275)
local bHopHigh = criarBtn("Auto Server Hop 100K/500K/1M: OFF", 335)
local bHopAll = criarBtn("Auto Trocar Servidor: OFF", 395)

criarLabel("Visuales (ESP)", 465)
local bESP = criarBtn("ESP Nombres: OFF", 495)

-- LOGICA
local savedPos = nil
local noclip, autoK, hopHigh, hopAll, espOn = false,false,false,false,false
local autoStealEnabled = true -- sempre ligado por baixo

-- salvar pos
bSave.MouseButton1Click:Connect(function()
    savedPos = LP.Character.HumanoidRootPart.CFrame
    bSave.Text = "✅ Posición Guardada!"
    task.wait(1) bSave.Text = "Guardar Posición"
end)
bTP.MouseButton1Click:Connect(function()
    if savedPos then LP.Character.HumanoidRootPart.CFrame = savedPos end
end)

-- NoClip
bNo.MouseButton1Click:Connect(function()
    noclip = not noclip
    if noclip then bNo.Text = "NoClip: ON" bNo.BackgroundColor3 = Color3.fromRGB(35,85,55)
    else bNo.Text = "NoClip: OFF" bNo.BackgroundColor3 = Color3.fromRGB(45,47,55) end
end)

RS.Stepped:Connect(function()
    if noclip then pcall(function() for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end) end
end)

-- Auto Kitar com K
bKitar.MouseButton1Click:Connect(function()
    autoK = not autoK
    bKitar.Text = autoK and "Auto Kitar [K]: ON" or "Auto Kitar [K]: OFF"
    bKitar.BackgroundColor3 = autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
end)

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode == Enum.KeyCode.K then
        if autoK then
            -- solta o palhaço
            pcall(function()
                local tool = LP.Character:FindFirstChildWhichIsA("Tool")
                if tool then
                    tool.Parent = LP.Backpack
                end
                -- remote de kitar
                for _,r in pairs(game.ReplicatedStorage:GetDescendants()) do
                    if r:IsA("RemoteEvent") and r.Name:lower():find("drop") then r:FireServer() end
                end
            end)
        end
    end
end)

-- Auto Kitar automatico quando segura
task.spawn(function()
    while true do task.wait(0.3)
        if autoK then pcall(function()
            if LP.Character:FindFirstChildWhichIsA("Tool") then
                local myPlot
                for _,p in pairs(workspace.Plots:GetChildren()) do
                    if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then myPlot = p break end
                end
                if myPlot and (LP.Character.HumanoidRootPart.Position - myPlot:GetPivot().Position).Magnitude < 15 then
                    task.wait(0.5)
                    local tool = LP.Character:FindFirstChildWhichIsA("Tool")
                    if tool then tool.Parent = LP.Backpack end
                end
            end
        end) end
    end
end)

-- SERVER HOP 100K/500K/1M
bHopHigh.MouseButton1Click:Connect(function()
    hopHigh = not hopHigh
    bHopHigh.Text = hopHigh and "Auto Server Hop 100K/500K/1M: ON" or "Auto Server Hop 100K/500K/1M: OFF"
    bHopHigh.BackgroundColor3 = hopHigh and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
end)

bHopAll.MouseButton1Click:Connect(function()
    hopAll = not hopAll
    bHopAll.Text = hopAll and "Auto Trocar Servidor: ON" or "Auto Trocar Servidor: OFF"
    bHopAll.BackgroundColor3 = hopAll and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
end)

-- ESP + AUTO ROUBO INTELIGENTE 100K+
task.spawn(function()
    while true do task.wait(1)
        if hopHigh or hopAll or autoStealEnabled then pcall(function()
            local foundHigh = false
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                    for _,model in pairs(plot:GetChildren()) do
                        if model:IsA("Model") and model:FindFirstChild("PricePerSecond") then
                            local v = model.PricePerSecond.Value
                            if v >= 100000 then foundHigh = true end -- 100k+
                            if hopHigh and v >= 100000 then
                                -- achou 100k/500k/1M, para de trocar
                                hopHigh = false
                                bHopHigh.Text = "Auto Server Hop 100K/500K/1M: OFF"
                                bHopHigh.BackgroundColor3 = Color3.fromRGB(45,47,55)
                                -- começa roubar esse
                                LP.Character.HumanoidRootPart.CFrame = model.PrimaryPart.CFrame + Vector3.new(0,0,2)
                            end
                        end
                    end
                end
            end
            if hopHigh and not foundHigh then
                task.wait(5)
                TS:Teleport(game.PlaceId, LP)
            end
            if hopAll and not foundHigh then
                task.wait(8)
                TS:Teleport(game.PlaceId, LP)
            end
        end) end
    end
end)

-- ESP Nombres
bESP.MouseButton1Click:Connect(function()
    espOn = not espOn
    bESP.Text = espOn and "ESP Nombres: ON" or "ESP Nombres: OFF"
    bESP.BackgroundColor3 = espOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
end)

task.spawn(function()
    while true do task.wait(1.5)
        if espOn then pcall(function()
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                for _,model in pairs(plot:GetChildren()) do
                    if model:IsA("Model") and model:FindFirstChild("PricePerSecond") and not model:FindFirstChild("ESP_MIIIGUEX") then
                        local bill = Instance.new("BillboardGui", model)
                        bill.Name = "ESP_MIIIGUEX"
                        bill.Size = UDim2.new(0,200,0,50)
                        bill.StudsOffset = Vector3.new(0,4,0)
                        bill.AlwaysOnTop = true
                        local lbl = Instance.new("TextLabel", bill)
                        lbl.Size = UDim2.new(1,0,1,0)
                        lbl.BackgroundTransparency = 1
                        lbl.Text = model.Name.."\n$"..model.PricePerSecond.Value.."/s"
                        lbl.TextColor3 = Color3.fromRGB(0,255,0)
                        lbl.TextStrokeTransparency = 0
                        lbl.Font = Enum.Font.GothamBold
                        lbl.TextSize = 12
                    end
                end
            end
        end) end
    end
end)

-- AUTO ROUBAR BASE (por baixo, sempre tentando)
task.spawn(function()
    while true do task.wait(0.2)
        pcall(function()
            if LP.Character:FindFirstChildWhichIsA("Tool") then return end
            local best = nil
            local bestVal = 0
            for _,plot in pairs(workspace.Plots:GetChildren()) do
                if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                    for _,m in pairs(plot:GetChildren()) do
                        if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                            if m.PricePerSecond.Value > bestVal then
                                bestVal = m.PricePerSecond.Value
                                best = m
                            end
                        end
                    end
                end
            end
            if best then
                LP.Character.HumanoidRootPart.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,0,1.5)
                task.wait(0.15)
                firetouchinterest(LP.Character.HumanoidRootPart, best.PrimaryPart, 0)
                task.wait(0.08)
                firetouchinterest(LP.Character.HumanoidRootPart, best.PrimaryPart, 1)
            end
        end)
    end
end)
