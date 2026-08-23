-- MIIIGUEX V18 - AUTO ROUBAR SÓ 100K+
-- Só rouba palhaço de 100K pra cima

local LP = game.Players.LocalPlayer
local RS = game:GetService("RunService")
local TS = game:GetService("TeleportService")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 360, 0, 280)
main.Position = UDim2.new(0.5,-180,0.5,-140)
main.BackgroundColor3 = Color3.fromRGB(24,24,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,16)

local function btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0,15,0,y)
    b.Size = UDim2.new(1,-30,0,52)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(45,47,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 14
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,12)
    return b
end

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,40)
title.Text = "MIIIGUEX V18 - SÓ 100K+"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 15

local bNo = btn("NoClip: OFF", 45)
local bAuto = btn("🤡 AUTO ROUBAR 100K+: OFF", 105, Color3.fromRGB(130,30,30))
local bHop = btn("🔄 HOP SE NÃO TIVER 100K: OFF", 165)
local bClose = btn("Cerrar", 225, Color3.fromRGB(60,60,60))

local noclip, auto, hop = false,false,false

bNo.MouseButton1Click:Connect(function() 
    noclip = not noclip 
    bNo.Text = noclip and "NoClip: ON" or "NoClip: OFF"
    bNo.BackgroundColor3 = noclip and Color3.fromRGB(0,170,80) or Color3.fromRGB(45,47,55)
end)

bAuto.MouseButton1Click:Connect(function() 
    auto = not auto 
    bAuto.Text = auto and "🤡 AUTO ROUBAR 100K+: ON" or "🤡 AUTO ROUBAR 100K+: OFF"
    bAuto.BackgroundColor3 = auto and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30)
end)

bHop.MouseButton1Click:Connect(function() 
    hop = not hop 
    bHop.Text = hop and "🔄 HOP SE NÃO TIVER 100K: ON" or "🔄 HOP SE NÃO TIVER 100K: OFF"
    bHop.BackgroundColor3 = hop and Color3.fromRGB(0,170,80) or Color3.fromRGB(45,47,55)
end)

bClose.MouseButton1Click:Connect(function() gui:Destroy() end)

RS.Stepped:Connect(function()
    if noclip or auto then 
        pcall(function() 
            for _,v in pairs(LP.Character:GetDescendants()) do 
                if v:IsA("BasePart") then v.CanCollide = false end 
            end 
        end) 
    end
end)

local function getMyPlot()
    for _,p in pairs(workspace:WaitForChild("Plots"):GetChildren()) do
        if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then return p end
    end
end

-- LOOP SÓ 100K+
task.spawn(function()
    while true do task.wait(0.3)
        if auto then pcall(function()
            local char = LP.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if not hrp then return end

            -- se tá com palhaço, leva pra base e kita
            if char:FindFirstChildWhichIsA("Tool") then
                local my = getMyPlot()
                if my then
                    hrp.CFrame = my:GetPivot() * CFrame.new(0,6,0)
                    task.wait(0.7)
                    local tool = char:FindFirstChildWhichIsA("Tool")
                    if tool then tool.Parent = LP.Backpack end
                end
            else
                -- PROCURA SÓ 100K OU MAIS
                local best = nil
                local bestVal = 0
                for _,plot in pairs(workspace.Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                        for _,m in pairs(plot:GetChildren()) do
                            if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                                local val = m.PricePerSecond.Value
                                if val >= 100000 and val > bestVal then -- AQUI É O FILTRO 100K+
                                    bestVal = val
                                    best = m
                                end
                            end
                        end
                    end
                end

                if best then
                    hrp.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,0,2)
                    task.wait(0.2)
                    firetouchinterest(hrp, best.PrimaryPart, 0)
                    task.wait(0.1)
                    firetouchinterest(hrp, best.PrimaryPart, 1)
                    for _,d in pairs(best:GetDescendants()) do
                        if d:IsA("ProximityPrompt") then fireproximityprompt(d) end
                        if d:IsA("ClickDetector") then fireclickdetector(d) end
                    end
                    game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Tentando roubar 100K+: "..best.Name.." $"..bestVal.."/s", Duration=2})
                else
                    -- não tem nenhum 100K no server
                    if hop then
                        game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Nenhum 100K+ aqui, trocando...", Duration=2})
                        task.wait(3)
                        TS:Teleport(game.PlaceId, LP)
                    end
                end
            end
        end) end
    end
end)
