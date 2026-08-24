-- MIIIGUEX HUB - TESTE LOOP FIX
getgenv().MIIIGUEX_DATA = getgenv().MIIIGUEX_DATA or {autoHop=true}
getgenv().Visitados = getgenv().Visitados or {}

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Plots = workspace:WaitForChild("Plots")
local SG = game.StarterGui

local function ServerHop()
    SG:SetCore("SendNotification",{Title="MIIIGUEX HUB", Text="Trocando!", Duration=2})
    pcall(function() TS:Teleport(game.PlaceId, LP) end)
end

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,250) main.Position = UDim2.new(0.5,-190,0.5,-125) main.BackgroundColor3 = Color3.fromRGB(24,24,28) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,60,0,60) openBtn.Position=UDim2.new(0,15,0.5,-30) openBtn.Text="M" openBtn.BackgroundColor3=Color3.fromRGB(25,25,25) openBtn.TextColor3=Color3.new(1,1,1) openBtn.Font=Enum.Font.GothamBold openBtn.TextSize=24 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30)

local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,44) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(45,47,55) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=12 Instance.new("UICorner", b).CornerRadius=UDim.new(0,12) return b end
local bStatus=btn("STATUS: FARM ON",20,Color3.fromRGB(35,85,55))
local bCont=btn("ROUBADOS: 0",75,Color3.fromRGB(30,30,50))
local bClose=btn("FECHAR [M] - R PAUSA",130,Color3.fromRGB(80,40,40))

local noclip=true local savedCFrame=nil local roubados=0 local pausado=false

local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible end
local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,8,0) end end end
local function temPalhaco100k() local melhor,valor=nil,0 for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") and m:FindFirstChild("PricePerSecond").Value>=100000 then if m.PricePerSecond.Value>valor then valor=m.PricePerSecond.Value melhor=m end end end end end return melhor,valor end

bClose.MouseButton1Click:Connect(toggleGUI) openBtn.MouseButton1Click:Connect(toggleGUI)
UIS.InputBegan:Connect(function(i,gp) if gp then return end if i.KeyCode==Enum.KeyCode.M then toggleGUI() elseif i.KeyCode==Enum.KeyCode.R then pausado=not pausado bStatus.Text=pausado and "PAUSADO" or "STATUS: FARM ON" end end)
RS.Stepped:Connect(function() pcall(function() local char = LP.Character if not char then return end if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end char:FindFirstChildOfClass("Humanoid").WalkSpeed=150 end) end)

-- LOOP INFINITO CORRIGIDO - NUNCA PARA
task.spawn(function()
    task.wait(4)
    local basePos=getBase() if basePos then savedCFrame=CFrame.new(basePos) end
    SG:SetCore("SendNotification",{Title="MIIIGUEX HUB", Text="LOOP FIX ATIVADO - Farm infinito!", Duration=4})

    while true do
        task.wait(0.2)
        if pausado then bStatus.Text="PAUSADO [R]" task.wait(0.5) continue end

        pcall(function()
            local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end
            local tool=char:FindFirstChildWhichIsA("Tool")

            if tool then
                -- COM PALHAÇO NA MÃO -> VOLTA BASE
                local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost") then return end
                bStatus.Text="ROUBOU! Voltando base..."
                if savedCFrame then hrp.CFrame=savedCFrame end
                task.wait(0.5)
                if char:FindFirstChildWhichIsA("Tool") then
                    char:FindFirstChildWhichIsA("Tool").Parent=LP.Backpack
                    roubados=roubados+1
                    bCont.Text="ROUBADOS: "..roubados
                    SG:SetCore("SendNotification",{Title="💰 ROUBOU!", Text="Total: "..roubados.." | Indo pro próximo", Duration=3})
                    task.wait(1.5)
                end
            else
                -- SEM NADA NA MÃO -> PROCURA
                local best,val = temPalhaco100k()
                if best then
                    bStatus.Text="LOOP: Achou $"..val.." TPANDO"
                    hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2)
                    task.wait(0.15)
                    firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1)
                    for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end
                    task.wait(0.5) -- tenta de novo se não pegou
                else
                    bStatus.Text="LOOP: Sem 100K+ trocando em 3s..."
                    task.wait(3)
                    if not pausado then
                        ServerHop()
                        task.wait(5)
                    end
                end
            end
        end)
    end
end)
