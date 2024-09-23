local selectedParts = {}
local isHighlighting = false

local function highlightParts()
    for _, part in pairs(selectedParts) do
        part.BrickColor = BrickColor.Green()
    end
end

local function unhighlightParts()
    for _, part in pairs(selectedParts) do
        part.BrickColor = BrickColor.White()
    end
end

local function toggleHighlighting()
    isHighlighting = not isHighlighting
    if isHighlighting then
        highlightParts()
    else
        unhighlightParts()
    end
end

local function selectPart(part)
    table.insert(selectedParts, part)
    if isHighlighting then
        part.BrickColor = BrickColor.Green()
    end
end

local function deselectPart(part)
    table.remove(selectedParts, table.find(selectedParts, part))
    if isHighlighting then
        part.BrickColor = BrickColor.White()
    end
end

-- Create the GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.CoreGui

local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.new(0.1, 0, 0.1, 0)
frame.Position = UDim2.new(0.9, 0, 0.9, 0)
frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

local offButton = Instance.new("TextButton")
offButton.Parent = frame
offButton.Text = "Off"
offButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
offButton.TextColor3 = Color3.fromRGB(255, 255, 255)
offButton.MouseButton1Click:Connect(toggleHighlighting)

local onButton = Instance.new("TextButton")
onButton.Parent = frame
onButton.Text = "On"
onButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
onButton.TextColor3 = Color3.fromRGB(255, 255, 255)
onButton.MouseButton1Click:Connect(toggleHighlighting)

-- Connect event handlers for selecting and deselecting parts
game.Workspace.DescendantsAdded:Connect(function(child)
    if child:IsA("Part") then
        child.MouseClick:Connect(function()
            if table.find(selectedParts, child) then
                deselectPart(child)
            else
                selectPart(child)
            end
        end)
    end
end)
