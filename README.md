-- Mx V1 - ALL IN ONE + FARM 100K+ AFK
getgenv().MxConfig = getgenv().MxConfig or {
    minValue=100000,
    onlyGod=false,
    onlySecret=false,
    espOn=true,
    autoLock=true,
    autoCollect=true,
    webhook="",
    blacklist={}
}

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Plots = workspace:WaitForChild("Plots")
local SG = game.StarterGui
local Http = game:GetService("HttpService")

local function notify(t,m) pcall(function() SG:SetCore("SendNotification",{Title=t, Text=m, Duration=3}) end) end
local function ServerHop() notify("Mx V1","Trocando de server...") pcall(function() TS:Teleport(game.PlaceId, LP) end) end

pcall(function() LP.PlayerGui:FindFirstChild("Mx_V1"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name="Mx_V1" gui.ResetOnSpawn=false
local main = Instance.new("Frame", gui) main.Size=UDim2.new(0,400,0,580) main.Position=UDim2.new(0.5,-200,0.5,-290) main.BackgroundColor3=Color3.fromRGB(18,18,22) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius=UDim.new(0,18)
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,65,0,65) openBtn.Position=UDim2.new(0,15,0.5,-32) openBtn.Text="Mx" openBtn.Visible=false openBtn.BackgroundColor3=Color3.fromRGB(120,40,200) openBtn.TextColor3=Color3.new(1,1,1) openBtn.Font=Enum.Font.GothamBold openBtn.TextSize=20 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30)

local function title(t,y) local l=Instance.new("TextLabel", main) l.Position=UDim2.new(0,15,0,y) l.Size=UDim2.new(1,-30,0,20) l.BackgroundTransparency=1 l.Text=t l.TextColor3=Color3.fromRGB(150,150,170) l.Font=Enum.Font.GothamBold l.TextSize=11 l.TextXAlignment=Enum.TextXAlignment.Left return l end
local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,38) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(35,35,45) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=11 Instance.new("UICorner", b).CornerRadius=UDim.new(0,10) return b end

title("Mx V1 - BRAINROT FARM",10)
local bStatus=btn("STATUS: Procurando...",35,Color3.fromRGB(25,25,35))
local bCont=btn("ROUBADOS: 0 | $/s: 0",78,Color3.fromRGB(30,30,60))

title("FARM CONFIG",125)
local bMin=btn("MINIMO: $100K",145,Color3.fromRGB(45,45,80))
local bGod=btn("SÓ GOD: OFF",188)
local bSecret=btn("SÓ SECRET: OFF",231)

title("PROTEÇÃO & VISUAL",278)
local bLock=btn("AUTO LOCK BASE: ON",298,Color3.fromRGB(35,85,55))
local bCollect=btn("AUTO COLETAR: ON",341,Color3.fromRGB(35,85,55))
local bEsp=btn("ESP 100K+: ON",384,Color3.fromRGB(35,85,55))
local bSpeed=btn("SPEED 150 + Noclip: ON",427,Color3.fromRGB(35,85,55))
local bClose=btn("FECHAR [M] | PAUSA [R]",470,Color3.fromRGB(80,35,35))

local savedCFrame=nil local roubados=0 local pausado=false local noclip=true local speedOn=true local totalPerSec=0

local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible end
local function getBasePlot() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p end end end
local function getBase() local p=getBasePlot() if p then return p:GetPivot().Position+Vector3.new(0,8,0) end end

-- ESP
local espFolder=Instance.new("Folder", workspace) espFolder.Name="MxESP"
local function clearEsp() espFolder:ClearAllChildren() end
local function addEsp(model, val) if not getgenv().MxConfig.espOn then return end local bb=Instance.new("BillboardGui", espFolder) bb.Adornee=model.PrimaryPart bb.Size=UDim2.new(0,120,0,50) bb.StudsOffset=Vector3.new(0,4,0) bb.AlwaysOnTop=true local tl=Instance.new("TextLabel", bb) tl.Size=UDim2.new(1,0,1,0) tl.BackgroundTransparency=1 tl.Text=model.Name.."\n$"..val tl.TextColor3=Color3.fromRGB(0,255,130) tl.Font=Enum.Font.GothamBold tl.TextSize=12 tl.TextStrokeTransparency=0.5 local hl=Instance.new("Highlight", espFolder) hl.Adornee=model hl.FillColor=Color3.fromRGB(0,255,130) hl.OutlineColor=Color3.new(1,1,1) end

local function checkFilter(m)
    if not m:FindFirstChild("PricePerSecond") then return false end
    local v=m.PricePerSecond.Value
    if v < getgenv().MxConfig.minValue then return false end
    if getgenv().MxConfig.onlyGod and not string.find(string.lower(m.Name),"god") then return false end
    if getgenv().MxConfig.onlySecret and not string.find(string.lower(m.Name),"secret") then return false end
    return true
end

local function temMelhor()
    clearEsp()
    local melhor,valor=nil,0
    totalPerSec=0
    local basePlot=getBasePlot()
    if basePlot then for _,m in pairs(basePlot:GetChildren()) do if m:IsA("Model") and m:FindFirstChild("PricePerSecond") then totalPerSec=totalPerSec+m.PricePerSecond.Value end end end
    bCont.Text="ROUBADOS: "..roubados.." | MEU $/s: "..totalPerSec

    for _,plot in pairs(Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then
            if table.find(getgenv().MxConfig.blacklist, plot.Owner.Value) then continue end
            for _,m in pairs(plot:GetChildren()) do
                if m:IsA("Model") and m.PrimaryPart and checkFilter(m) then
                    addEsp(m, m.PricePerSecond.Value)
                    if m.PricePerSecond.Value > valor then valor=m.PricePerSecond.Value melhor=m end
                end
            end
        end
    end
    return melhor,valor
end

-- Botões
bMin.MouseButton1Click:Connect(function()
    if getgenv().MxConfig.minValue==100000 then getgenv().MxConfig.minValue=200000 bMin.Text="MINIMO: $200K"
    elseif getgenv().MxConfig.minValue==200000 then getgenv().MxConfig.minValue=500000 bMin.Text="MINIMO: $500K"
    else getgenv().MxConfig.minValue=100000 bMin.Text="MINIMO: $100K" end
end)
bGod.MouseButton1Click:Connect(function() getgenv().MxConfig.onlyGod=not getgenv().MxConfig.onlyGod bGod.Text=getgenv().MxConfig.onlyGod and "SÓ GOD: ON" or "SÓ GOD: OFF" bGod.BackgroundColor3=getgenv().MxConfig.onlyGod and Color3.fromRGB(35,85,55) or Color3.fromRGB(35,35,45) end)
bSecret.MouseButton1Click:Connect(function() getgenv().MxConfig.onlySecret=not getgenv().MxConfig.onlySecret bSecret.Text=getgenv().MxConfig.onlySecret and "SÓ SECRET: ON" or "SÓ SECRET: OFF" bSecret.BackgroundColor3=getgenv().MxConfig.onlySecret and Color3.fromRGB(35,85,55) or Color3.fromRGB(35,35,45) end)
bLock.MouseButton1Click:Connect(function() getgenv().MxConfig.autoLock=not getgenv().MxConfig.autoLock bLock.Text=getgenv().MxConfig.autoLock and "AUTO LOCK BASE: ON" or "AUTO LOCK BASE: OFF" bLock.BackgroundColor3=getgenv().MxConfig.autoLock and Color3.fromRGB(35,85,55) or Color3.fromRGB(35,35,45) end)
bCollect.MouseButton1Click:Connect(function() getgenv().MxConfig.autoCollect=not getgenv().MxConfig.autoCollect bCollect.Text=getgenv().MxConfig.autoCollect and "AUTO COLETAR: ON" or "AUTO COLETAR: OFF" bCollect.BackgroundColor3=getgenv().MxConfig.autoCollect and Color3.fromRGB(35,85,55) or Color3.fromRGB(35,35,45) end)
bEsp.MouseButton1Click:Connect(function() getgenv().MxConfig.espOn=not getgenv().MxConfig.espOn bEsp.Text=getgenv().MxConfig.espOn and "ESP 100K+: ON" or "ESP 100K+: OFF" bEsp.BackgroundColor3=getgenv().MxConfig.espOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(35,35,45) if not getgenv().MxConfig.espOn then clearEsp() end end)
bSpeed.MouseButton1Click:Connect(function() speedOn=not speedOn noclip=not noclip bSpeed.Text=speedOn and "SPEED 150 + Noclip: ON" or "SPEED 150 + Noclip: OFF" end)
bClose.MouseButton1Click:Connect(toggleGUI) openBtn.MouseButton1Click:Connect(toggleGUI)
UIS.InputBegan:Connect(function(i,gp) if gp then return end if i.KeyCode==Enum.KeyCode.M then toggleGUI() elseif i.KeyCode==Enum.KeyCode.R then pausado=not pausado bStatus.Text=pausado and "PAUSADO [R]" or "STATUS: Farmando..." end end)

RS.Stepped:Connect(function() pcall(function()
    local char=LP.Character if not char then return end
    if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end
    if speedOn then local h=char:FindFirstChildOfClass("Humanoid") if h then h.WalkSpeed=150 end end
    if getgenv().MxConfig.autoCollect then
        local bp=getBasePlot()
        if bp then for _,v in pairs(bp:GetDescendants()) do if v:IsA("TouchTransmitter") and v.Parent.Name=="Collect" then firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 0) firetouchinterest(LP.Character.HumanoidRootPart, v.Parent, 1) end end end
    end
    if getgenv().MxConfig.autoLock then
        local bp=getBasePlot()
        if bp then for _,v in pairs(bp:GetDescendants()) do if v:IsA("ProximityPrompt") and string.find(string.lower(v.ObjectText),"lock") then if v.Enabled then fireproximityprompt(v) end end end end
    end
end) end)

-- LOOP PRINCIPAL AFK
task.spawn(function()
    task.wait(4)
    local basePos=getBase() if basePos then savedCFrame=CFrame.new(basePos) end
    notify("Mx V1","Mx V1 iniciado! Farm AFK ON")
    while true do task.wait(0.15)
        if pausado then continue end
        pcall(function()
            local char=LP.Character local hrp=char and char:FindFirstChild("HumanoidRootPart") if not hrp then return end
            local tool=char:FindFirstChildWhichIsA("Tool")
            if tool then
                if string.find(string.lower(tool.Name),"cloak") then return end
                bStatus.Text="Voltando pra base..."
                if savedCFrame then hrp.CFrame=savedCFrame end
                task.wait(0.4)
                if char:FindFirstChildWhichIsA("Tool") then
                    char:FindFirstChildWhichIsA("Tool").Parent=LP.Backpack
                    roubados=roubados+1
                    notify("💰 Mx V1 ROUBOU!","Total: "..roubados)
                    task.wait(1)
                end
            else
                local best,val=temMelhor()
                if best then
                    bStatus.Text="Roubando: $"..val.." - "..best.Name
                    hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2)
                    task.wait(0.15)
                    firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1)
                    for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end
                else
                    bStatus.Text="Sem alvo, trocando em 3s..."
                    task.wait(3)
                    if not pausado then ServerHop() task.wait(5) end
                end
            end
        end)
    end
end)
