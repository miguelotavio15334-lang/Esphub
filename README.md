-- COLOCA DENTRO DA TOOL > HANDLE > Script

local tool = script.Parent.Parent
local handle = script.Parent

local HITBOX_SIZE = Vector3.new(15, 10, 20) -- AQUI TU MUDA O TAMANHO, quanto maior mais roubado
local DAMAGE = 15
local canHit = true

tool.Activated:Connect(function()
    if not canHit then return end
    canHit = false

    local char = tool.Parent
    if not char:FindFirstChild("HumanoidRootPart") then return end

    -- Mostra a hitbox (só pra teste, depois pode tirar)
    local box = Instance.new("Part")
    box.Size = HITBOX_SIZE
    box.CFrame = handle.CFrame * CFrame.new(0,0,-HITBOX_SIZE.Z/2)
    box.Anchored = true
    box.CanCollide = false
    box.Transparency = 0.7
    box.Color = Color3.fromRGB(255,0,0)
    box.Parent = workspace
    game.Debris:AddItem(box, 0.3)

    local params = OverlapParams.new()
    params.FilterType = Enum.RaycastFilterType.Exclude
    params.FilterDescendantsInstances = {char}

    local parts = workspace:GetPartBoundsInBox(box.CFrame, HITBOX_SIZE, params)
    local jaHitou = {}

    for _,part in pairs(parts) do
        local enemy = part.Parent
        if enemy:FindFirstChild("Humanoid") and not jaHitou[enemy] then
            jaHitou[enemy] = true
            enemy.Humanoid:TakeDamage(DAMAGE)

            -- empurrãozinho
            if enemy:FindFirstChild("HumanoidRootPart") then
                local bv = Instance.new("BodyVelocity", enemy.HumanoidRootPart)
                bv.Velocity = (enemy.HumanoidRootPart.Position - char.HumanoidRootPart.Position).Unit * 25 + Vector3.new(0,5,0)
                bv.MaxForce = Vector3.new(1e5,1e5,1e5)
                game.Debris:AddItem(bv, 0.2)
            end
        end
    end

    task.wait(0.5)
    canHit = true
end)
