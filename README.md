-- MIIIGUEX V16.5 - X SALVA TP / Q USA TP + TELEPORT BASE
getgenv().MIIIGUEX_DATA = getgenv().MIIIGUEX_DATA or {
    hopHigh = false,
    noclip = false,
    autoK = false,
    guiHidden = false,
    savedTP = nil
}

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Plots = workspace:WaitForChild("Plots")

local queueteleport = queue_on_teleport or (syn and syn.queue_on_teleport)
if queueteleport then
    queueteleport([[
        getgenv().MIIIGUEX_DATA = ]]..game:GetService("HttpService"):JSONEncode(getgenv().MIIIGUEX_DATA)..[[
        wait(4)
        loadstring(game:HttpGet("https://pastebin.com/raw/SEU_LINK_AQUI"))()
    ]])
end

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 380, 0, 560)
main.Position = UDim2.new(0.5,-190,0.5,-280)
main.BackgroundColor3 = Color3.fromRGB(24,24,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)

local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.new(0,50,0,50)
openBtn.Position = UDim2.new(0,10,0,10)
openBtn.Text = "M"
openBtn.Visible = getgenv().MIIIGUEX_DATA.guiHidden
openBtn.BackgroundColor3 = Color3.fromRGB(35,85,55)
openBtn.TextColor3 = Color3.new(1,1,1)
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 20
Instance.new("UICorner", openBtn).CornerRadius = UDim.new(0,25)

local function btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0,15,0,y)
    b.Size = UDim2.new(1,-30,0,48)
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
title.Text = "MIIIGUEX V16.5\nX=Salva TP | Q=Usa TP | M=Esconde"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 12

local bNo = btn("NoClip: OFF", 50)
local bKitar = btn("Auto Kitar [K]: OFF", 103)
local bTP = btn("TELEPORT PRA BASE AO PEGAR: ON", 156, Color3.fromRGB(35,85,55))
local bSave = btn("💾 SALVAR POS [X]: NENHUMA", 209)
local bUse = btn("📍 USAR TP SALVO [Q]", 262, Color3.fromRGB(60,60,90))
local bHopHigh = btn("Hop 100K+: OFF", 315)
local bClose = btn("Cerrar [M]", 368, Color3.fromRGB(60,60,60))

local noclip = getgenv().MIIIGUEX_DATA.noclip
local autoK = getgenv().MIIIGUEX_DATA.autoK
local hopHigh = getgenv().MIIIGUEX_DATA.hopHigh
local tpBase = true
local savedCFrame = getgenv().MIIIGUEX_DATA.savedTP and CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) or nil

main.Visible = not getgenv().MIIIGUEX_DATA.guiHidden
if savedCFrame then bSave.Text = "💾 SALVAR POS [X]: SALVO!" bSave.BackgroundColor3 = Color3.fromRGB(35,85,55) end

local function update()
    bNo.Text = noclip and "NoClip: ON" or "NoClip: OFF"
    bNo.BackgroundColor3 = noclip and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
    bKitar.Text = autoK and "Auto Kitar [K]: ON" or "Auto Kitar [K]: OFF"
    bKitar.BackgroundColor3 = autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
    bHopHigh.Text = hopHigh and "Hop 100K+: ON" or "Hop 100K+: OFF"
    bHopHigh.BackgroundColor3 = hopHigh and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
end
update()

local function salvar()
    getgenv().MIIIGUEX_DATA.noclip = noclip
    getgenv().MIIIGUEX_DATA.autoK = autoK
    getgenv().MIIIGUEX_DATA.hopHigh = hopHigh
end

local function toggleGUI()
    main.Visible = not main.Visible
    openBtn.Visible = not main.Visible
    getgenv().MIIIGUEX_DATA.guiHidden = not main.Visible
end

local function salvarPos()
    local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        savedCFrame = hrp.CFrame
        getgenv().MIIIGUEX_DATA.savedTP = {savedCFrame:GetComponents()}
        bSave.Text = "💾 SALVAR POS [X]: SALVO! "..string.format("%.0f, %.0f, %.0f", savedCFrame.X, savedCFrame.Y, savedCFrame.Z)
        bSave.BackgroundColor3 = Color3.fromRGB(35,85,55)
        game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Posição salva! Aperta Q pra voltar", Duration=2})
    end
end

local function usarPos()
    if savedCFrame then
        local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.CFrame = savedCFrame
            game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Teleportado pro lugar salvo!", Duration=2})
        end
    else
        game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Nenhuma posição salva! Aperta X", Duration=2})
    end
end

bNo.MouseButton1Click:Connect(function() noclip = not noclip salvar() update() end)
bKitar.MouseButton1Click:Connect(function() autoK = not autoK salvar() update() end)
bTP.MouseButton1Click:Connect(function() tpBase = not tpBase bTP.Text = tpBase and "TELEPORT PRA BASE AO PEGAR: ON" or "TELEPORT PRA BASE AO PEGAR: OFF" bTP.BackgroundColor3 = tpBase and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bSave.MouseButton1Click:Connect(function() salvarPos() end)
bUse.MouseButton1Click:Connect(function() usarPos() end)
bHopHigh.MouseButton1Click:Connect(function() hopHigh = not hopHigh salvar() update() end)
bClose.MouseButton1Click:Connect(function() toggleGUI() end)
openBtn.MouseButton1Click:Connect(function() toggleGUI() end)

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode == Enum.KeyCode.M then toggleGUI()
    elseif i.KeyCode == Enum.KeyCode.X then salvarPos()
    elseif i.KeyCode == Enum.KeyCode.Q then usarPos()
    elseif i.KeyCode == Enum.KeyCode.K and autoK then
        pcall(function() local t = LP.Character:FindFirstChildWhichIsA("Tool") if t then t.Parent = LP.Backpack end end)
    end
end)

RS.Stepped:Connect(function()
    if noclip or tpBase then
        pcall(function() for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end)
    end
end)

local function getBase()
    for _,p in pairs(Plots:GetChildren()) do
        if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then
            return p:GetPivot().Position + Vector3.new(0,5,0)
        end
    end
end

task.spawn(function()
    while true do task.wait(0.1)
        pcall(function()
            local char = LP.Character
            if not char then return end
            local hrp = char:FindFirstChild("HumanoidRootPart")
            if not hrp then return end

            local tool = char:FindFirstChildWhichIsA("Tool")
            if tool and tpBase then
                local basePos = getBase()
                if basePos then
                    hrp.CFrame = CFrame.new(basePos)
                    task.wait(0.3)
                    local t = char:FindFirstChildWhichIsA("Tool") if t then t.Parent = LP.Backpack end
                end
            elseif not tool then
                local best, bestVal = nil, 0
                for _,plot in pairs(Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                        for _,m in pairs(plot:GetChildren()) do
                            if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                                local v = m.PricePerSecond.Value
                                if v >= 100000 and v > bestVal then bestVal = v best = m end
                            end
                        end
                    end
                end
                if best then
                    hrp.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,0,2)
                    task.wait(0.15)
                    firetouchinterest(hrp, best.PrimaryPart, 0)
                    firetouchinterest(hrp, best.PrimaryPart, 1)
                    for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end
                elseif hopHigh then task.wait(2) TS:Teleport(game.PlaceId, LP) end
            end
        end)
    end
end)
