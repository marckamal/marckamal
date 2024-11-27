-- Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local DataStoreService = game:GetService("DataStoreService")

local player = Players.LocalPlayer

-- Setup GUI for the Dragon Blox Control Panel
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "DragonBloxControlPanel"
ScreenGui.Parent = player:WaitForChild("PlayerGui")

-- Main Frame for Control Panel
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 500, 0, 900)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -450)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Parent = ScreenGui

-- Title for the Control Panel
local Title = Instance.new("TextLabel")
Title.Text = "Dragon Blox Control Panel"
Title.Size = UDim2.new(1, 0, 0.1, 0)
Title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextScaled = true
Title.Parent = MainFrame

-- Ultimate Gold Zenny Button
local function grantUltimateGoldZenny()
    local zennyStore = DataStoreService:GetDataStore("UltimateGoldZenny")
    local success, errorMsg = pcall(function()
        local currentZenny = zennyStore:GetAsync(player.UserId) or 0
        local newZenny = currentZenny + 1_000_000 -- Grant 1 million zenny
        zennyStore:SetAsync(player.UserId, newZenny)
        print("Ultimate Gold Zenny granted! New balance:", newZenny)
    end)

    if not success then
        warn("Failed to grant zenny:", errorMsg)
    end
end

local UltimateGoldZennyButton = Instance.new("TextButton")
UltimateGoldZennyButton.Text = "Get Ultimate Gold Zenny"
UltimateGoldZennyButton.Size = UDim2.new(0, 300, 0, 50)
UltimateGoldZennyButton.Position = UDim2.new(0.5, -150, 0.1, 0)
UltimateGoldZennyButton.BackgroundColor3 = Color3.fromRGB(255, 215, 0)
UltimateGoldZennyButton.TextColor3 = Color3.fromRGB(0, 0, 0)
UltimateGoldZennyButton.Parent = MainFrame
UltimateGoldZennyButton.MouseButton1Click:Connect(grantUltimateGoldZenny)

-- World and Mob/Boss Selection
local worlds = {
    ["Main World"] = {"Puriza", "Coolest", "Droid 17", "Droid 18", "Jinbu", "Baby Veggy", "Black Karrot", "Brawly", "Jigray"},
    ["Crimson World"] = {"Gordo", "Calci", "Truffle"},
    ["Planet Droid"] = {"Droid King", "Mecha Droid", "Robo Boss"}
}

local function createWorldSelection()
    local DropdownFrame = Instance.new("Frame")
    DropdownFrame.Size = UDim2.new(0, 400, 0, 500)
    DropdownFrame.Position = UDim2.new(0.5, -200, 0.2, 0)
    DropdownFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    DropdownFrame.Parent = MainFrame

    for world, mobs in pairs(worlds) do
        local WorldButton = Instance.new("TextButton")
        WorldButton.Text = world
        WorldButton.Size = UDim2.new(1, 0, 0, 40)
        WorldButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
        WorldButton.Parent = DropdownFrame
        WorldButton.MouseButton1Click:Connect(function()
            print("Selected World: " .. world)

            -- Create Mob/Boss Dropdown
            local MobDropdown = Instance.new("Frame")
            MobDropdown.Size = UDim2.new(1, 0, 0, #mobs * 30)
            MobDropdown.Position = UDim2.new(0, 0, 1, 0)
            MobDropdown.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            MobDropdown.Parent = WorldButton

            for i, mob in ipairs(mobs) do
                local MobButton = Instance.new("TextButton")
                MobButton.Text = mob
                MobButton.Size = UDim2.new(1, 0, 0, 30)
                MobButton.Position = UDim2.new(0, 0, 0, (i - 1) * 30)
                MobButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
                MobButton.Parent = MobDropdown
                MobButton.MouseButton1Click:Connect(function()
                    print("Selected Mob/Boss: " .. mob)
                end)
            end
        end)
    end
end

local WorldSelectionButton = Instance.new("TextButton")
WorldSelectionButton.Text = "Select World & Mob/Boss"
WorldSelectionButton.Size = UDim2.new(0, 300, 0, 50)
WorldSelectionButton.Position = UDim2.new(0.5, -150, 0.3, 0)
WorldSelectionButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
WorldSelectionButton.Parent = MainFrame
WorldSelectionButton.MouseButton1Click:Connect(createWorldSelection)

-- Additional Functionalities Buttons
local features = {
    {"Toggle Auto-Farming", "Activates auto-farming on selected mobs or bosses."},
    {"Toggle Auto-Kill Mobs", "Auto-kills mobs in the selected area."},
    {"Toggle Auto-Kill Bosses", "Auto-kills bosses like Puriza, Coolest, and more."},
    {"Toggle Auto-Quest", "Automatically accepts and completes quests."},
    {"Toggle Auto-Rebirth", "Automatically rebirths after reaching max level."},
    {"Toggle Anti-AFK", "Prevents being kicked for inactivity."},
    {"Toggle Loot Pickup", "Automatically collects loot."},
    {"Toggle Orb Collection", "Collects orbs from the world."},
    {"Toggle Physical Block", "Enhances physical block damage reduction."},
}

for i, feature in ipairs(features) do
    local Button = Instance.new("TextButton")
    Button.Text = feature[1]
    Button.Size = UDim2.new(0, 300, 0, 50)
    Button.Position = UDim2.new(0.5, -150, 0.4 + (i - 1) * 0.1, 0)
    Button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    Button.Parent = MainFrame
    Button.MouseButton1Click:Connect(function()
        print(feature[2] .. " toggled!")
    end)
end
