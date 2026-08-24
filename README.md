-- MIIIGUEX V18.2 PC - TATU FIXADO EMBAIXO
getgenv().MIIIGUEX_DATA = getgenv().MIIIGUEX_DATA or {hopHigh=false,noclip=true,autoK=true,guiHidden=true,savedTP=nil,speed=true,tpAuto=true}
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

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,680) main.Position = UDim2.new(0.5,-190,0.5,-340) main.BackgroundColor3 = Color3.fromRGB(24,24,28) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,60,0,60) openBtn.Position=UDim2.new(0,15,0.5,-30) openBtn.Text="M" openBtn.Visible=true openBtn.BackgroundColor3=Color3.fromRGB(25,25,25) openBtn.TextColor3=Color3.new(1,1,1) openBtn.Font=Enum.Font.GothamBold openBtn.TextSize=24 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30) local stroke = Instance.new("UIStroke", openBtn) stroke.Thickness=2 stroke.Color=Color3.fromRGB(0,255,100)

local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,44) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(45,47,55) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=12 Instance.new("UICorner", b).CornerRadius=UDim.new(0,12) return b end

local bSpeed=btn("SPEED 150: ON",50,Color3.fromRGB(35,85,55))
local bNo=btn("NoClip: ON",98,Color3.fromRGB(35,85,55))
local bKitar=btn("Auto Kitar [K]: ON",146,Color3.fromRGB(35,85,55))
local bTP=btn("Q AUTO AO PEGAR: ON",194,Color3.fromRGB(35,85,55))
local bSave=btn("💾 SALVAR BASE [X]",242)
local bUse=btn("📍 USAR TP [Q]",290,Color3.fromRGB(60,60,90))
local bHopHigh=btn("Hop 100K+: OFF",338)
local bTatu=btn("MODO TATU [T]: OFF",386,Color3.fromRGB(200,50,50))
local bClose=btn("FECHAR [M]",434,Color3.fromRGB(80,40,40))

local noclip=true local autoK=true local hopHigh=false local tpAuto=true local speedOn=true local tatuOn=false local tatuPos=nil local tatuCFrame=nil
local savedCFrame=getgenv().MIIIGUEX_DATA.savedTP and CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) or nil

main.Visible = false openBtn.Visible = true
if savedCFrame then bSave.Text="💾 X: SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end

local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end
local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame getgenv().MIIIGUEX_DATA.savedTP={savedCFrame:GetComponents()} bSave.Text="💾 X SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end end
local function usarPos() if tatuOn and tatuCFrame then LP.Character.HumanoidRootPart.CFrame=tatuCFrame elseif savedCFrame then LP.Character.HumanoidRootPart.CFrame=savedCFrame end end

local function entrarTatu()
    local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    if not tatuPos then tatuPos = hrp.CFrame end
    noclip = true
    tatuCFrame = CFrame.new(hrp.Position.X, hrp.Position.Y - 35, hrp.Position.Z) * CFrame.Angles(math.rad(180),0,0)
    savedCFrame = tatuCFrame
    getgenv().MIIIGUEX_DATA.savedTP = {tatuCFrame:GetComponents()}
    tatuOn = true
    bTatu.Text = "MODO TATU [T]: ON - TRAVADO EMBAIXO!"
    bTatu.BackgroundColor3 = Color3.fromRGB(35,85,55)
    -- TELEPORTA 3 VEZES PRA GARANTIR
    for i=1,3 do hrp.CFrame = tatuCFrame task.wait(0.1) end
    game.StarterGui:SetCore("SendNotification",{Title="MODO TATU",Text="Travado 35 blocos embaixo! De ponta cabeça",Duration=3})
end

local function sairTatu()
    tatuOn = false tatuCFrame = nil
    bTatu.Text = "MODO TATU [T]: OFF" bTatu.BackgroundColor3 = Color3.fromRGB(200,50,50)
    local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if hrp and tatuPos then hrp.CFrame = tatuPos + Vector3.new(0,5,0) end
    tatuPos = nil
end

bSpeed.MouseButton1Click:Connect(function() speedOn=not speedOn bSpeed.Text=speedOn and "SPEED 150: ON" or "SPEED 16: OFF" bSpeed.BackgroundColor3=speedOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bNo.MouseButton1Click:Connect(function() noclip=not noclip bNo.Text=noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3=noclip and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bKitar.MouseButton1Click:Connect(function() autoK=not autoK bKitar.Text=autoK and "Auto Kitar [K]: ON" or "Auto Kitar [K]: OFF" bKitar.BackgroundColor3=autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bTP.MouseButton1Click:Connect(function() tpAuto=not tpAuto bTP.Text=tpAuto and "Q AUTO AO PEGAR: ON" or "Q AUTO AO PEGAR: OFF" bTP.BackgroundColor3=tpAuto and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bSave.MouseButton1Click:Connect(salvarPos) bUse.MouseButton1Click:Connect(usarPos)
bHopHigh.MouseButton1Click:Connect(function() hopHigh=not hopHigh bHopHigh.Text=hopHigh and "Hop 100K+: ON" or "Hop 100K+: OFF" bHopHigh.BackgroundColor3=hopHigh and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bTatu.MouseButton1Click:Connect(function() if tatuOn then sairTatu() else entrarTatu() end end)
bClose.MouseButton1Click:Connect(toggleGUI) openBtn.MouseButton1Click:Connect(toggleGUI)

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode==Enum.KeyCode.M then toggleGUI()
    elseif i.KeyCode==Enum.KeyCode.X then salvarPos()
    elseif i.KeyCode==Enum.KeyCode.Q then usarPos()
    elseif i.KeyCode==Enum.KeyCode.T then if tatuOn then sairTatu() else entrarTatu() end
    elseif i.KeyCode==Enum.KeyCode.K and autoK then
        pcall(function()
            local t=LP.Character:FindFirstChildWhichIsA("Tool")
            if t then local n=string.lower(t.Name) if not (string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost")) then t.Parent=LP.Backpack end end
        end)
    end
end)

RS.Stepped:Connect(function()
    pcall(function()
        local char = LP.Character if not char then return end
        if noclip or tatuOn then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end
        if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end
        -- TRAVA EMBAIXO SE TATU LIGADO
        if tatuOn and tatuCFrame then
            local hrp = char:FindFirstChild("HumanoidRootPart")
            if hrp and (hrp.Position.Y > tatuCFrame.Position.Y + 10) then
                hrp.CFrame = tatuCFrame
            end
        end
    end)
end)

task.spawn(function()
    task.wait(2)
    if getgenv().MIIIGUEX_DATA.savedTP and not tatuOn then savedCFrame=CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end
    pcall(function() LP.Character.Humanoid.WalkSpeed=150 end)
end)

task.spawn(function()
    while true do task.wait(0.08)
        pcall(function()
            if tatuOn then return end -- NÃO ROUBA ENQUANTO TATU, SÓ FICA ESCONDIDO. SE QUISER ROUBAR TATU, TIRA ESSA LINHA
            local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end
            local tool=char:FindFirstChildWhichIsA("Tool")
            if tool then
                local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost") then return end
                if tpAuto and savedCFrame then hrp.CFrame=savedCFrame task.wait(0.35) local t=char:FindFirstChildWhichIsA("Tool") if t then local nn=string.lower(t.Name) if not (string.find(nn,"cloak") or string.find(nn,"invis") or string.find(nn,"cape") or string.find(nn,"ghost")) then t.Parent=LP.Backpack end end end
            else
                local best,bestVal=nil,0
                for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") and m.PricePerSecond.Value>=100000 and m.PricePerSecond.Value>bestVal then bestVal=m.PricePerSecond.Value best=m end end end end
                if best then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.15) firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end elseif hopHigh then task.wait(2) TS:Teleport(game.PlaceId,LP) end
            end
        end)
    end
end)
game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX V18.2 PC", Text="M=menu | T=tatu | X=salva", Duration=5})
