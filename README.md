local player = game:GetService("Players").LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Ensure the PrimaryPart is set correctly
if not character:FindFirstChild("HumanoidRootPart") then
    warn("HumanoidRootPart not found!")
    return
end

-- First CFrame coordinates (These will be ignored if the PlaceId matches)
local cframe1 = CFrame.new(-373.323212, 21.5559349, 2933.05786, -0.694649816, 0, 0.719348073, 0, 1, 0, -0.719348073, 0, -0.694649816)
-- Second CFrame coordinates (These will also be ignored if the PlaceId matches)
local cframe2 = CFrame.new(-411.809052, 21.5559349, 2970.22314, 0.694649816, 0, -0.719348073, 0, 1, 0, 0.719348073, 0, 0.694649816)

-- Function to teleport and fire remotes if PlaceId doesn't match
local function teleportAndFireRemotes()
    -- Check if the PlaceId matches
    if game.PlaceId ~= 17720215900 then
        -- Teleport to the first CFrame
        print("Teleporting to first CFrame...")
        character:SetPrimaryPartCFrame(cframe1)
        task.wait(5) -- Wait for 5 seconds before moving to the second CFrame

        -- Teleport to the second CFrame
        print("Teleporting to second CFrame...")
        character:SetPrimaryPartCFrame(cframe2)
        task.wait(1) -- Small delay before firing the remotes

        -- Fire the first remote (ChooseStage)
        local args1 = {
            [1] = workspace.Teleporters.Teleporter5,
            [2] = 15,
            [3] = "Hellmode",
            [4] = false
        }

        local chooseStageRemote = game:GetService("ReplicatedStorage").Remotes.Teleporters:FindFirstChild("ChooseStage")
        if chooseStageRemote then
            chooseStageRemote:FireServer(unpack(args1))
            print("Fired ChooseStage remote")
        else
            warn("ChooseStage remote not found!")
        end

        -- Wait for 5 seconds before firing the next remote
        task.wait(3)

        -- Fire the second remote (Start)
        local args2 = {
            [1] = workspace.Teleporters.Teleporter5
        }

        local startRemote = game:GetService("ReplicatedStorage").Remotes.Teleporters:FindFirstChild("Start")
        if startRemote then
            startRemote:FireServer(unpack(args2))
            print("Fired Start remote")
        else
            warn("Start remote not found!")
        end
    else
        -- PlaceId matched, loading content from the URL
        print("PlaceId matched, loading content from the URL...")
        loadstring(game:HttpGet("https://raw.githubusercontent.com/omaromar242/Megumi-units/refs/heads/main/README.md"))()
    end
end

-- Start the teleporting and remote firing process
teleportAndFireRemotes()
