-- MIIIGUEX V19 - FOCADO NOS DA FOTO
-- Alfa Payachini 600K, Ian 175K, Luli y Fede 150K

local LP = game.Players.LocalPlayer
local RS = game:GetService("RunService")
local TS = game:GetService("TeleportService")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 380, 0, 310)
main.Position = UDim2.new(0.5,-190,0.5,-155)
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
    b.TextSize = 13
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,12)
    return b
end

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,45)
title.Text = "MIIIGUEX V19 - FOTO TARGET\nAlfa 600K | Ian 175K | Luli 150K"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 12

local bNo = btn("NoClip: OFF", 50)
local bAuto = btn("🤡 ROUBAR ESSES 3: OFF", 110, Color3.fromRGB(130,30,30))
local bHop = btn("🔄 HOP ATÉ ACHAR ESSES 3: OFF", 170)
local bClose = btn("Cerrar", 230, Color3.fromRGB(60,60,60))

local noclip, auto, hop = false,false,false
bNo.MouseButton1Click:Connect(function() noclip = not noclip bNo.Text = noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3 = noclip and Color3.fromRGB(0,170,80) or Color3.fromRGB(45,47,55) end)
bAuto.MouseButton1Click:Connect(function() auto = not auto bAuto.Text = auto and "🤡 ROUBAR ESSES 3: ON" or "🤡 ROUBAR ESSES 3: OFF" bAuto.BackgroundColor3 = auto and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30) end)
bHop.MouseButton1Click:Connect(function() hop = not hop bHop.Text = hop and "🔄 HOP ATÉ ACHAR ESSES 3: ON" or "🔄 HOP ATÉ ACHAR ESSES 3: OFF" bHop.BackgroundColor3 = hop and Color3.fromRGB(0,170,80) or Color3.fromRGB(45,47,55) end)
bClose.MouseButton1Click:Connect(function() gui:Destroy() end)

RS.Stepped:Connect(function()
    if noclip or auto then pcall(function() for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end) end
end)

local function getMyPlot()
    for _,p in pairs(workspace:WaitForChild("Plots"):GetChildren()) do
        if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then return p end
    end
end

-- NOMES EXATOS DA TUA FOTO
local TARGETS = {
    ["Alfa Payachini Malini"] = true,
    ["Alfa Payachini"] = true,
    ["Ian"] = true,
    ["Ian Brainrot God"] = true,
    ["Brainrot God"] = true,
    ["Luli y Fede"] = true,
    ["Luli y Fede Brainrot God"] = true,
}

task.spawn(function()
    while true do task.wait(0.25)
        if auto then pcall(function()
            local char = LP.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if not hrp then return end

            if char:FindFirstChildWhichIsA("Tool") then
                local my = getMyPlot()
                if my then
                    hrp.CFrame = my:GetPivot() * CFrame.new(0,6,0)
                    task.wait(0.6)
                    local tool = char:FindFirstChildWhichIsA("Tool")
                    if tool then tool.Parent = LP.Backpack end
                end
            else
                local best = nil
                local bestVal = 0
                
                for _,plot in pairs(workspace.Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                        for _,m in pairs(plot:GetChildren()) do
                            if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                                local val = m.PricePerSecond.Value
                                -- SÓ PEGA SE FOR 150K+ E FOR UM DOS DA FOTO
                                local isTarget = false
                                for name,_ in pairs(TARGETS) do
                                    if m.Name:lower():find(name:lower()) or (m:FindFirstChild("DisplayName") and m.DisplayName.Value:lower():find(name:lower())) then
                                        isTarget = true
                                        break
                                    end
                                end
                                
                                -- Se o valor for 150K+ já considera GOD igual da foto
                                if val >= 150000 then isTarget = true end
                                
                                if isTarget and val > bestVal then
                                    bestVal = val
                                    best = m
                                end
                            end
                        end
                    end
                end

                if best then
                    hrp.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,0,1.5)
                    task.wait(0.15)
                    -- TENTA TUDO
                    firetouchinterest(hrp, best.PrimaryPart, 0)
                    task.wait(0.08)
                    firetouchinterest(hrp, best.PrimaryPart, 1)
                    for _,d in pairs(best:GetDescendants()) do
                        if d:IsA("ProximityPrompt") then
                            d.HoldDuration = 0
                            fireproximityprompt(d)
                        end
                        if d:IsA("ClickDetector") then fireclickdetector(d) end
                    end
                    game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Roubando: "..best.Name.." $"..bestVal.."/s", Duration=2})
                else
                    if hop then
                        task.wait(2)
                        TS:Teleport(game.PlaceId, LP)
                    end
                end
            end
        end) end
    end
end)
