-- MIIIGUEX V17 - MODO FANTASMA (JÁ ENTRA ESCONDIDO + TUDO ON)
getgenv().MIIIGUEX_DATA = getgenv().MIIIGUEX_DATA or {
    hopHigh = false,
    noclip = true,
    autoK = true,
    guiHidden = true, -- JÁ ENTRA ESCONDIDO
    savedTP = nil,
    speed = true,
    tpAuto = true,
    firstRun = true
}

-- FORÇA TUDO ON
getgenv().MIIIGUEX_DATA.guiHidden = true
getgenv().MIIIGUEX_DATA.noclip = true
getgenv().MIIIGUEX_DATA.speed = true
getgenv().MIIIGUEX_DATA.autoK = true
getgenv().MIIIGUEX_DATA.tpAuto = true

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Plots = workspace:WaitForChild("Plots")

local queueteleport = queue_on_teleport or (syn and syn.queue_on_teleport)
if queueteleport then queueteleport([[getgenv().MIIIGUEX_DATA = ]]..game:GetService("HttpService"):JSONEncode(getgenv().MIIIGUEX_DATA)..[[ wait(4) loadstring(game:HttpGet("https://pastebin.com/raw/SEU_LINK_AQUI"))()]]) end

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,620) main.Position = UDim2.new(0.5,-190,0.5,-310) main.BackgroundColor3 = Color3.fromRGB(24,24,28) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)
local openBtn = Instance.new("TextButton", gui)
openBtn.Size=UDim2.new(0,60,0,60)
openBtn.Position=UDim2.new(0,15,0.5,-30)
openBtn.Text="M"
openBtn.Visible=true -- M SEMPRE VISÍVEL NO INÍCIO
openBtn.BackgroundColor3=Color3.fromRGB(25,25,25)
openBtn.TextColor3=Color3.new(1,1,1)
openBtn.Font=Enum.Font.GothamBold
openBtn.TextSize=24
openBtn.Transparency=0
Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30)
local stroke = Instance.new("UIStroke", openBtn) stroke.Thickness=2 stroke.Color=Color3.fromRGB(0,255,100)

local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,44) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(45,47,55) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=12 Instance.new("UICorner", b).CornerRadius=UDim.new(0,12) return b end

local bSpeed=btn("SPEED 150: ON",50,Color3.fromRGB(35,85,55))
local bNo=btn("NoClip: ON",98,Color3.fromRGB(35,85,55))
local bKitar=btn("Auto Kitar [K]: ON",146,Color3.fromRGB(35,85,55))
local bTP=btn("Q AUTO AO PEGAR: ON",194,Color3.fromRGB(35,85,55))
local bSave=btn("💾 SALVAR BASE [X]",242)
local bUse=btn("📍 USAR TP [Q]",290,Color3.fromRGB(60,60,90))
local bHopHigh=btn("Hop 100K+: OFF",338)
local bClose=btn("FECHAR [M] - MODO FANTASMA",386,Color3.fromRGB(80,40,40))

local noclip=true local autoK=true local hopHigh=false local tpAuto=true local speedOn=true
local savedCFrame=getgenv().MIIIGUEX_DATA.savedTP and CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) or nil

-- JÁ COMEÇA ESCONDIDO
main.Visible = false
openBtn.Visible = true

if savedCFrame then bSave.Text="💾 X: SALVO! Q AUTO LIGADO" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end

local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible getgenv().MIIIGUEX_DATA.guiHidden=not main.Visible end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame getgenv().MIIIGUEX_DATA.savedTP={savedCFrame:GetComponents()} bSave.Text="💾 X SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX",Text="Base salva no X!",Duration=2}) end end
local function usarPos() if savedCFrame then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end end

bSpeed.MouseButton1Click:Connect(function() speedOn=not speedOn getgenv().MIIIGUEX_DATA.speed=speedOn bSpeed.Text=speedOn and "SPEED 150: ON" or "SPEED 16: OFF" bSpeed.BackgroundColor3=speedOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) if not speedOn then pcall(function() LP.Character.Humanoid.WalkSpeed=16 end) end end)
bNo.MouseButton1Click:Connect(function() noclip=not noclip bNo.Text=noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3=noclip and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bKitar.MouseButton1Click:Connect(function() autoK=not autoK bKitar.Text=autoK and "Auto Kitar [K]: ON" or "Auto Kitar [K]: OFF" bKitar.BackgroundColor3=autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bTP.MouseButton1Click:Connect(function() tpAuto=not tpAuto bTP.Text=tpAuto and "Q AUTO AO PEGAR: ON" or "Q AUTO AO PEGAR: OFF" bTP.BackgroundColor3=tpAuto and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bSave.MouseButton1Click:Connect(function() salvarPos() end) bUse.MouseButton1Click:Connect(function() usarPos() end) bHopHigh.MouseButton1Click:Connect(function() hopHigh=not hopHigh getgenv().MIIIGUEX_DATA.hopHigh=hopHigh bHopHigh.Text=hopHigh and "Hop 100K+: ON" or "Hop 100K+: OFF" bHopHigh.BackgroundColor3=hopHigh and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bClose.MouseButton1Click:Connect(function() toggleGUI() end) openBtn.MouseButton1Click:Connect(function() toggleGUI() end)
UIS.InputBegan:Connect(function(i,gp) if gp then return end if i.KeyCode==Enum.KeyCode.M then toggleGUI() elseif i.KeyCode==Enum.KeyCode.X then salvarPos() elseif i.KeyCode==Enum.KeyCode.Q then usarPos() elseif i.KeyCode==Enum.KeyCode.K and autoK then pcall(function() local t=LP.Character:FindFirstChildWhichIsA("Tool") if t then t.Parent=LP.Backpack end end) end end)

RS.Stepped:Connect(function()
    pcall(function()
        local char = LP.Character if not char then return end
        if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end
        if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end
    end)
end)
LP.CharacterAdded:Connect(function(char) task.wait(1) if speedOn then local hum=char:WaitForChild("Humanoid") hum.WalkSpeed=150 end end)
local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end

-- AUTO X 1x AO ENTRAR + JÁ ESCONDIDO
task.spawn(function()
    task.wait(3.5)
    if getgenv().MIIIGUEX_DATA.savedTP then
        savedCFrame = CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP))
        local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.CFrame = savedCFrame end
    else
        local basePos = getBase()
        if basePos then savedCFrame = CFrame.new(basePos) getgenv().MIIIGUEX_DATA.savedTP = {savedCFrame:GetComponents()} bSave.Text = "💾 X AUTO: BASE SALVA" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end
    end
    pcall(function() LP.Character.Humanoid.WalkSpeed = 150 end)
end)

task.spawn(function() while true do task.wait(0.08) pcall(function() local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end local tool=char:FindFirstChildWhichIsA("Tool") if tool and tpAuto then if savedCFrame then hrp.CFrame=savedCFrame else local basePos=getBase() if basePos then hrp.CFrame=CFrame.new(basePos) end end task.wait(0.35) local t=char:FindFirstChildWhichIsA("Tool") if t then t.Parent=LP.Backpack end elseif not tool then local best,bestVal=nil,0 for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") and m.PricePerSecond.Value>=100000 and m.PricePerSecond.Value>bestVal then bestVal=m.PricePerSecond.Value best=m end end end end if best then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.15) firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end elseif hopHigh then task.wait(2) TS:Teleport(game.PlaceId,LP) end end end) end end)

game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX V17 FANTASMA", Text="TUDO ON e ESCONDIDO! Aperta M", Duration=5})
