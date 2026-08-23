-- MIIIGUEX V13 ULTRA - ROBA UN PAYASO FIX
local LP = game.Players.LocalPlayer
local RS = game:GetService("RunService")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 320, 0, 350)
main.Position = UDim2.new(0.5,-160,0.5,-175)
main.BackgroundColor3 = Color3.fromRGB(18,18,22)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)

local function Btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Size = UDim2.new(0.9,0,0,45)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(35,40,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 13
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    return b
end

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,45)
title.Text = "Roba un payaso — MIIIGUEX V13"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local bNo = Btn("NoClip: OFF", 50)
local bAuto = Btn("🤡 AUTO ROUBAR: OFF", 105, Color3.fromRGB(130,30,30))
local bSell = Btn("💰 AUTO VENDER: OFF", 160, Color3.fromRGB(130,30,30))
local bHop = Btn("🔄 AUTO SERVER HOP: OFF", 215, Color3.fromRGB(130,30,30))
local bOff = Btn("Cerrar", 270, Color3.fromRGB(60,60,60))

local noclip, auto, autosell, autohop = false,false,false,false

bNo.MouseButton1Click:Connect(function() noclip = not noclip bNo.Text = noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3 = noclip and Color3.fromRGB(0,170,80) or Color3.fromRGB(35,40,55) end)
bAuto.MouseButton1Click:Connect(function() auto = not auto bAuto.Text = auto and "🤡 AUTO ROUBAR: ON" or "🤡 AUTO ROUBAR: OFF" bAuto.BackgroundColor3 = auto and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30) end)
bSell.MouseButton1Click:Connect(function() autosell = not autosell bSell.Text = autosell and "💰 AUTO VENDER: ON" or "💰 AUTO VENDER: OFF" bSell.BackgroundColor3 = autosell and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30) end)
bHop.MouseButton1Click:Connect(function() autohop = not autohop bHop.Text = autohop and "🔄 AUTO SERVER HOP: ON" or "🔄 AUTO SERVER HOP: OFF" bHop.BackgroundColor3 = autohop and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30) end)
bOff.MouseButton1Click:Connect(function() gui:Destroy() end)

RS.Stepped:Connect(function()
    if noclip or auto then
        pcall(function()
            for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
        end)
    end
end)

-- FUNÇÃO QUE PEGA O PLOT DO JOGADOR
local function getMyPlot()
    for _,plot in pairs(workspace:WaitForChild("Plots"):GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then return plot end
    end
    return nil
end

-- AUTO ROUBAR REAL
task.spawn(function()
    while true do task.wait(0.15)
        if auto then pcall(function()
            local char = LP.Character
            local hrp = char.HumanoidRootPart
            -- se já tá com palhaço
            local tool = char:FindFirstChildWhichIsA("Tool")
            if tool then
                local myPlot = getMyPlot()
                if myPlot then
                    -- teleport pra base e segura até soltar
                    hrp.CFrame = myPlot:GetPivot() * CFrame.new(0,5,0)
                end
            else
                -- procura palhaço mais caro
                local target = nil
                local bestValue = 0
                for _,plot in pairs(workspace.Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                        for _,model in pairs(plot:GetChildren()) do
                            if model:IsA("Model") and model:FindFirstChild("PricePerSecond") then
                                -- PEGA TODOS, não filtra mais
                                local v = model.PricePerSecond.Value
                                if v > bestValue then
                                    bestValue = v
                                    target = model
                                end
                            end
                        end
                    end
                end
                if target and target.PrimaryPart then
                    hrp.CFrame = target.PrimaryPart.CFrame + Vector3.new(0,0,2)
                    task.wait(0.2)
                    -- metodo que funciona nesse jogo
                    firetouchinterest(hrp, target.PrimaryPart, 0)
                    task.wait(0.1)
                    firetouchinterest(hrp, target.PrimaryPart, 1)
                    -- tenta click também
                    for _,d in pairs(target:GetDescendants()) do
                        if d:IsA("ProximityPrompt") then fireproximityprompt(d) end
                        if d:IsA("ClickDetector") then fireclickdetector(d) end
                    end
                else
                    if autohop then
                        task.wait(2)
                        game:GetService("TeleportService"):Teleport(game.PlaceId, LP)
                    end
                end
            end
        end) end
    end
end)

-- AUTO VENDER - nesse jogo é um botão na base chamado "Sell"
task.spawn(function()
    while true do task.wait(1)
        if autosell then pcall(function()
            local myPlot = getMyPlot()
            if not myPlot then return end
            for _,v in pairs(myPlot:GetDescendants()) do
                if v:IsA("TouchTransmitter") and v.Parent.Name:lower():find("sell") then
                    firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 0)
                    firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 1)
                end
                if v:IsA("ProximityPrompt") and v.ActionText:lower():find("sell") then
                    fireproximityprompt(v)
                end
            end
        end) end
    end
end)
