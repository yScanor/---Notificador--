--[[
🎯 Rastreador de Frutas 🍈
by yScanor
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local function findFruitsInWorkspace()
local fruits = {}
local function recursiveSearch(parent)
for _, child in pairs(parent:GetChildren()) do
if child:IsA("Model") and child:FindFirstChild("Handle") then
table.insert(fruits, child)
elseif child:IsA("Folder") or child:IsA("Model") then
recursiveSearch(child)
end
end
end
recursiveSearch(Workspace)
return fruits
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FruitTrackerGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 360, 0, 260)
MainFrame.Position = UDim2.new(0.05, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
MainFrame.BorderSizePixel = 0
MainFrame.AnchorPoint = Vector2.new(0, 0)
MainFrame.Visible = true
MainFrame.Parent = ScreenGui
MainFrame.ClipsDescendants = true
MainFrame.BackgroundTransparency = 0
MainFrame.Name = "MainFrame"

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 14)

local UIStroke = Instance.new("UIStroke", MainFrame)
UIStroke.Color = Color3.fromRGB(55, 55, 60)
UIStroke.Thickness = 2
UIStroke.Transparency = 0.6

local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 38)
Title.BackgroundTransparency = 1
Title.Text = "🎯 Rastreador de Frutas 🍈"
Title.TextColor3 = Color3.fromRGB(230, 230, 230)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 24
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Position = UDim2.new(0, 16, 0, 8)
Title.TextStrokeTransparency = 0.75

local Credit = Instance.new("TextLabel")
Credit.Parent = MainFrame
Credit.Size = UDim2.new(1, -16, 0, 18)
Credit.Position = UDim2.new(0, 0, 1, -28)
Credit.BackgroundTransparency = 1
Credit.Text = "by yScanor"
Credit.TextColor3 = Color3.fromRGB(150, 150, 150)
Credit.Font = Enum.Font.Gotham
Credit.TextSize = 14
Credit.TextXAlignment = Enum.TextXAlignment.Right

local CloseButton = Instance.new("TextButton")
CloseButton.Parent = MainFrame
CloseButton.Size = UDim2.new(0, 28, 0, 28)
CloseButton.Position = UDim2.new(1, -38, 0, 6)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseButton.Text = "×"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 22
CloseButton.AutoButtonColor = true
CloseButton.Name = "CloseButton"
CloseButton.AnchorPoint = Vector2.new(0, 0)
CloseButton.BorderSizePixel = 0
CloseButton.ClipsDescendants = false
CloseButton.BackgroundTransparency = 0

local CloseCorner = Instance.new("UICorner", CloseButton)
CloseCorner.CornerRadius = UDim.new(0, 6)

local ControlsFrame = Instance.new("Frame")
ControlsFrame.Parent = MainFrame
ControlsFrame.Size = UDim2.new(1, -32, 0, 56)
ControlsFrame.Position = UDim2.new(0, 16, 0, 60)
ControlsFrame.BackgroundTransparency = 1

local SwitchLabel = Instance.new("TextLabel")
SwitchLabel.Parent = ControlsFrame
SwitchLabel.Size = UDim2.new(0, 130, 1, 0)
SwitchLabel.BackgroundTransparency = 1
SwitchLabel.Text = "ESP Fruta"
SwitchLabel.TextColor3 = Color3.fromRGB(230, 230, 230)
SwitchLabel.Font = Enum.Font.GothamBold
SwitchLabel.TextSize = 18
SwitchLabel.TextXAlignment = Enum.TextXAlignment.Left
SwitchLabel.Position = UDim2.new(0, 0, 0, 0)

local SwitchButton = Instance.new("TextButton")
SwitchButton.Parent = ControlsFrame
SwitchButton.Size = UDim2.new(0, 55, 0.7, 0)
SwitchButton.Position = UDim2.new(0, 140, 0.15, 0)
SwitchButton.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
SwitchButton.Text = "OFF"
SwitchButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SwitchButton.Font = Enum.Font.GothamBold
SwitchButton.TextSize = 16
SwitchButton.AutoButtonColor = false
SwitchButton.Name = "SwitchButton"
SwitchButton.AnchorPoint = Vector2.new(0, 0)
SwitchButton.BackgroundTransparency = 0
SwitchButton.ClipsDescendants = true

local SwitchCorner = Instance.new("UICorner", SwitchButton)
SwitchCorner.CornerRadius = UDim.new(0, 10)

local TPButton = Instance.new("TextButton")
TPButton.Parent = ControlsFrame
TPButton.Size = UDim2.new(0, 100, 0.7, 0)
TPButton.Position = UDim2.new(0, 200, 0.15, 0)
TPButton.BackgroundColor3 = Color3.fromRGB(55, 110, 220)
TPButton.Text = "TP Fruta"
TPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TPButton.Font = Enum.Font.GothamBold
TPButton.TextSize = 18
TPButton.AutoButtonColor = true
TPButton.Name = "TPButton"
TPButton.AnchorPoint = Vector2.new(0, 0)

local TPCorner = Instance.new("UICorner", TPButton)
TPCorner.CornerRadius = UDim.new(0, 10)

local ReopenCircle = Instance.new("TextButton")
ReopenCircle.Parent = ScreenGui
ReopenCircle.Size = UDim2.new(0, 60, 0, 60) -- Aumentado para 60x60
ReopenCircle.Position = UDim2.new(0, 14, 0.9, 0)
ReopenCircle.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
ReopenCircle.Text = "🍈"
ReopenCircle.TextColor3 = Color3.fromRGB(245, 245, 245)
ReopenCircle.Font = Enum.Font.GothamBold
ReopenCircle.TextSize = 38 -- Aumentado o texto também
ReopenCircle.AutoButtonColor = true
ReopenCircle.Visible = false
ReopenCircle.Name = "ReopenCircle"

local CircleCorner = Instance.new("UICorner", ReopenCircle)
CircleCorner.CornerRadius = UDim.new(1, 0)

local tracking = false
local trackedFruits = {}
local distanceLabels = {}
local highlightObjects = {}

local function createHighlight(part)
if highlightObjects[part] then return highlightObjects[part] end
local highlight = Instance.new("Highlight")
highlight.Adornee = part.Parent
highlight.FillColor = Color3.fromRGB(255, 50, 50)
highlight.OutlineColor = Color3.fromRGB(200, 0, 0)
highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
highlight.Parent = part.Parent
highlightObjects[part] = highlight
return highlight
end

local function removeHighlight(part)
if highlightObjects[part] then
highlightObjects[part]:Destroy()
highlightObjects[part] = nil
end
end

local function createOrUpdateDistanceLabel(fruit)
local part = fruit:FindFirstChild("Handle") or fruit.PrimaryPart or fruit:FindFirstChildWhichIsA("BasePart")
if not part then return end

local label = distanceLabels[fruit]  
if not label then  
    label = Instance.new("BillboardGui")  
    label.Adornee = part  
    label.Size = UDim2.new(0, 120, 0, 32)  
    label.AlwaysOnTop = true  
    label.StudsOffset = Vector3.new(0, 2.8, 0)  

    local textLabel = Instance.new("TextLabel")  
    textLabel.Parent = label  
    textLabel.BackgroundTransparency = 0.5  
    textLabel.BackgroundColor3 = Color3.fromRGB(20, 20, 20)  
    textLabel.Size = UDim2.new(1, 0, 1, 0)  
    textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)  
    textLabel.Font = Enum.Font.GothamBold  
    textLabel.TextSize = 15  
    textLabel.TextStrokeTransparency = 0  
    textLabel.TextWrapped = true  
    textLabel.TextYAlignment = Enum.TextYAlignment.Center  
    textLabel.Name = "DistanceText"  
    textLabel.ClipsDescendants = true  
    textLabel.BorderSizePixel = 0  
    textLabel.ZIndex = 10  

    label.Parent = ScreenGui  
    distanceLabels[fruit] = label  
end  

local distance = (HumanoidRootPart.Position - part.Position).Magnitude  
local textLabel = label:FindFirstChild("DistanceText")  
if textLabel then  
    textLabel.Text = string.format("%s\n%.2f m", fruit.Name, distance)  
end

end

local function removeDistanceLabel(fruit)
if distanceLabels[fruit] then
distanceLabels[fruit]:Destroy()
distanceLabels[fruit] = nil
end
end

local function clearAllTracking()
for fruit, _ in pairs(distanceLabels) do
removeDistanceLabel(fruit)
end
for part, _ in pairs(highlightObjects) do
removeHighlight(part)
end
end

local function updateTrackedFruits()
clearAllTracking()
trackedFruits = findFruitsInWorkspace()
end

updateTrackedFruits()

local targetFruit = nil
local isTeleporting = false

local function teleportToFruit(fruit)
if not fruit then return end
if isTeleporting then return end

local part = fruit:FindFirstChild("Handle") or fruit.PrimaryPart or fruit:FindFirstChildWhichIsA("BasePart")  
if not part then return end  

isTeleporting = true  
targetFruit = fruit  

local speed = 250  
local hrp = HumanoidRootPart  

while targetFruit and targetFruit.Parent and isTeleporting do  
    local targetPos = part.Position + Vector3.new(0, 5, 0)  
    local direction = (targetPos - hrp.Position).Unit  
    local distance = (targetPos - hrp.Position).Magnitude  

    if distance < 5 then  
        break  
    end  

    hrp.CFrame = hrp.CFrame + direction * math.min(speed * RunService.Heartbeat:Wait(), distance)  
end  

isTeleporting = false  
targetFruit = nil

end

-- Função para tentar armazenar automaticamente a fruta
local function tryPickFruit(fruit)
local part = fruit:FindFirstChild("Handle") or fruit.PrimaryPart or fruit:FindFirstChildWhichIsA("BasePart")
if not part then return end
local dist = (HumanoidRootPart.Position - part.Position).Magnitude
if dist <= 5 then
-- Aqui deve ser colocada a ação de pegar a fruta, que depende do jogo.
-- Exemplo:
-- fruit:Destroy()
-- Ou disparar evento do jogo que coleta a fruta.
-- Coloque o código correto de coleta da fruta aqui.
end
end

SwitchButton.MouseButton1Click:Connect(function()
tracking = not tracking
if tracking then
SwitchButton.Text = "ON"
SwitchButton.BackgroundColor3 = Color3.fromRGB(80, 220, 80) -- Verde quando ON
else
SwitchButton.Text = "OFF"
SwitchButton.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
clearAllTracking()
end
end)

TPButton.MouseButton1Click:Connect(function()
if not tracking then return end
local closestFruit = nil
local closestDistance = math.huge
for _, fruit in pairs(trackedFruits) do
local part = fruit:FindFirstChild("Handle") or fruit.PrimaryPart or fruit:FindFirstChildWhichIsA("BasePart")
if part and fruit.Parent then
local dist = (HumanoidRootPart.Position - part.Position).Magnitude
if dist < closestDistance then
closestDistance = dist
closestFruit = fruit
end
end
end
if closestFruit then
coroutine.wrap(function()
teleportToFruit(closestFruit)
end)()
end
end)

RunService.Heartbeat:Connect(function()
if tracking then
updateTrackedFruits()
for _, fruit in pairs(trackedFruits) do
local part = fruit:FindFirstChild("Handle") or fruit.PrimaryPart or fruit:FindFirstChildWhichIsA("BasePart")
if part and fruit.Parent then
createHighlight(part)
createOrUpdateDistanceLabel(fruit)
tryPickFruit(fruit) -- tenta armazenar automaticamente
else
removeHighlight(part)
removeDistanceLabel(fruit)
end
end
end
end)

CloseButton.MouseButton1Click:Connect(function()
MainFrame.Visible = false
ReopenCircle.Visible = true
end)

ReopenCircle.MouseButton1Click:Connect(function()
MainFrame.Visible = true
ReopenCircle.Visible = false
end)

-- Função para tornar GUI arrastável
local function makeDraggable(guiObject)
local dragging = false
local dragInput, mousePos, framePos

guiObject.InputBegan:Connect(function(input)  
    if input.UserInputType == Enum.UserInputType.MouseButton1 then  
        dragging = true  
        mousePos = input.Position  
        framePos = guiObject.Position  
        input.Changed:Connect(function()  
            if input.UserInputState == Enum.UserInputState.End then  
                dragging = false  
            end  
        end)  
    end  
end)  

guiObject.InputChanged:Connect(function(input)  
    if input.UserInputType == Enum.UserInputType.MouseMovement then  
        dragInput = input  
    end  
end)  

UserInputService.InputChanged:Connect(function(input)  
    if input == dragInput and dragging then  
        local delta = input.Position - mousePos  
        guiObject.Position = UDim2.new(framePos.X.Scale, framePos.X.Offset + delta.X, framePos.Y.Scale, framePos.Y.Offset + delta.Y)  
    end  
end)

end

makeDraggable(MainFrame)
makeDraggable(ReopenCircle)

