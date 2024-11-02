local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")

ScreenGui.Parent = game.CoreGui
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 200, 0, 50)
ToggleButton.Position = UDim2.new(0.5, -100, 0.1, 0)
ToggleButton.Text = "Auto Bounty OFF"
ToggleButton.BackgroundColor3 = Color3.new(1, 0, 0)
ToggleButton.TextColor3 = Color3.new(1, 1, 1)

getgenv().AutoBountyEnabled = false
getgenv().Settings = {
    ["AttackDistance"] = 50,
    ["Skills"] = {
        ["Melee"] = {["Enable"] = true, ["Z"] = true, ["X"] = true, ["C"] = true},
    }
}

function useSkills()
    local skills = getgenv().Settings["Skills"]
    if skills["Melee"]["Enable"] then
        if skills["Melee"]["Z"] then game.Players.LocalPlayer.Character:FindFirstChild("Melee").Z:FireServer() wait(1) end
        if skills["Melee"]["X"] then game.Players.LocalPlayer.Character:FindFirstChild("Melee").X:FireServer() wait(0.5) end
        if skills["Melee"]["C"] then game.Players.LocalPlayer.Character:FindFirstChild("Melee").C:FireServer() wait(0.5) end
    end
end

function autoBounty()
    while getgenv().AutoBountyEnabled do
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Team then
                if player.Team.Name == "Piratas" or player.Team.Name == "Marinheiros" then
                    local playerRoot = player.Character:FindFirstChild("HumanoidRootPart")
                    local myRoot = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                    
                    if playerRoot and myRoot then
                        local distance = (myRoot.Position - playerRoot.Position).Magnitude
                        if distance <= getgenv().Settings["AttackDistance"] then
                            useSkills()
                        end
                    end
                end
            end
        end
        wait(1)
    end
end

ToggleButton.MouseButton1Click:Connect(function()
    getgenv().AutoBountyEnabled = not getgenv().AutoBountyEnabled
    ToggleButton.Text = getgenv().AutoBountyEnabled and "Auto Bounty ON" or "Auto Bounty OFF"
    ToggleButton.BackgroundColor3 = getgenv().AutoBountyEnabled and Color3.new(0, 1, 0) or Color3.new(1, 0, 0)
    
    if getgenv().AutoBountyEnabled then
        autoBounty()
    end
end)
