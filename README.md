-- ============================================================
-- SCRIPT HUB : ค่ายรันดี (RunDee Hub) - Pure Lua UI Template
-- ============================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- สร้าง ScreenGui หลัก
local RunDeeGui = Instance.new("ScreenGui")
RunDeeGui.Name = "RunDeeHubUI"
RunDeeGui.ResetOnSpawn = false

if syn and syn.protect_gui then
    syn.protect_gui(RunDeeGui)
    RunDeeGui.Parent = game:GetService("CoreGui")
elseif gethui then
    RunDeeGui.Parent = gethui()
else
    RunDeeGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end

--------------------------------------------------------------------------------
-- 1. ปุ่มวงกลมสำหรับย่อ/เปิด UI (ลากขยับได้)
--------------------------------------------------------------------------------
local ToggleCircle = Instance.new("TextButton")
ToggleCircle.Name = "ToggleCircle"
ToggleCircle.Parent = RunDeeGui
ToggleCircle.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
ToggleCircle.Position = UDim2.new(0.05, 0, 0.2, 0)
ToggleCircle.Size = UDim2.new(0, 55, 0, 55)
ToggleCircle.Font = Enum.Font.FredokaOne
ToggleCircle.Text = "รันดี"
ToggleCircle.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleCircle.TextSize = 16
ToggleCircle.Active = true
ToggleCircle.Draggable = true

local CircleCorner = Instance.new("UICorner")
CircleCorner.CornerRadius = UDim.new(1, 0)
CircleCorner.Parent = ToggleCircle

local CircleStroke = Instance.new("UIStroke")
CircleStroke.Color = Color3.fromRGB(140, 60, 255)
CircleStroke.Thickness = 2.5
CircleStroke.Parent = ToggleCircle

--------------------------------------------------------------------------------
-- 2. หน้าต่าง UI หลัก (Main Frame)
--------------------------------------------------------------------------------
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = RunDeeGui
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
MainFrame.Position = UDim2.new(0.3, 0, 0.25, 0)
MainFrame.Size = UDim2.new(0, 480, 0, 320)
MainFrame.Active = true
MainFrame.Draggable = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(100, 50, 220)
MainStroke.Thickness = 1.5
MainStroke.Parent = MainFrame

-- แถบด้านบน (Header / Title Bar)
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Parent = MainFrame
TopBar.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
TopBar.Size = UDim2.new(1, 0, 0, 40)

local TopBarCorner = Instance.new("UICorner")
TopBarCorner.CornerRadius = UDim.new(0, 10)
TopBarCorner.Parent = TopBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = TopBar
TitleLabel.BackgroundTransparency = 1
TitleLabel.Position = UDim2.new(0, 15, 0, 0)
TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
TitleLabel.Font = Enum.Font.FredokaOne
TitleLabel.Text = "ค่ายรันดี Hub | by รันดี"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 18
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- ปุ่มย่อหน้าต่าง (ปุ่ม -)
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Parent = TopBar
MinimizeBtn.BackgroundTransparency = 1
MinimizeBtn.Position = UDim2.new(1, -35, 0, 0)
MinimizeBtn.Size = UDim2.new(0, 35, 1, 0)
MinimizeBtn.Font = Enum.Font.FredokaOne
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
MinimizeBtn.TextSize = 22

--------------------------------------------------------------------------------
-- 3. พื้นที่ใส่ข้อมูลและระบบ (Content Area - ScrollingFrame)
--------------------------------------------------------------------------------
local ContentContainer = Instance.new("ScrollingFrame")
ContentContainer.Name = "ContentContainer"
ContentContainer.Parent = MainFrame
ContentContainer.BackgroundTransparency = 1
ContentContainer.Position = UDim2.new(0, 10, 0, 50)
ContentContainer.Size = UDim2.new(1, -20, 1, -60)
ContentContainer.CanvasSize = UDim2.new(0, 0, 0, 0) -- สามารถปรับเพิ่มขนาดได้ตามความต้องการ
ContentContainer.ScrollBarThickness = 6
ContentContainer.ScrollBarImageColor3 = Color3.fromRGB(140, 60, 255)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ContentContainer
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)

--------------------------------------------------------------------------------
-- ฟังก์ชันช่วยสร้าง UI Element (สำหรับนำไปเรียกใช้งานง่ายขึ้น)
--------------------------------------------------------------------------------

-- ฟังก์ชันสร้างปุ่มกด
local function CreateButton(buttonText, callback)
    local Btn = Instance.new("TextButton")
    Btn.Parent = ContentContainer
    Btn.BackgroundColor3 = Color3.fromRGB(120, 50, 240)
    Btn.Size = UDim2.new(1, -10, 0, 40)
    Btn.Font = Enum.Font.SourceSansBold
    Btn.Text = buttonText
    Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    Btn.TextSize = 16
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Btn
    
    if callback then
        Btn.MouseButton1Click:Connect(callback)
    end
    
    -- อัปเดตขนาด Canvas อัตโนมัติเมื่อเพิ่มปุ่ม
    ContentContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
    return Btn
end

-- ฟังก์ชันสร้างป้ายข้อความ
local function CreateLabel(labelText)
    local Label = Instance.new("TextLabel")
    Label.Parent = ContentContainer
    Label.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
    Label.Size = UDim2.new(1, -10, 0, 30)
    Label.Font = Enum.Font.SourceSansBold
    Label.Text = "  " .. labelText
    Label.TextColor3 = Color3.fromRGB(180, 180, 255)
    Label.TextSize = 15
    Label.TextXAlignment = Enum.TextXAlignment.Left
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 6)
    Corner.Parent = Label
    
    ContentContainer.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
    return Label
end

--------------------------------------------------------------------------------
-- ระบบย่อ / เปิด หน้าต่าง UI
--------------------------------------------------------------------------------
local isMinimized = false

local function ToggleUI()
    isMinimized = not isMinimized
    MainFrame.Visible = not isMinimized
end

ToggleCircle.MouseButton1Click:Connect(ToggleUI)
MinimizeBtn.MouseButton1Click:Connect(ToggleUI)

--------------------------------------------------------------------------------
-- พื้นที่สำหรับเขียนโค้ดเพิ่มปุ่มหรือข้อมูลของคุณเองด้านล่างนี้
--------------------------------------------------------------------------------

-- ตัวอย่างการใช้งาน:
-- CreateLabel("หัวข้อของคุณ")
-- CreateButton("ชื่อปุ่มฟังก์ชัน", function()
--     -- โค้ดคำสั่งของคุณ
-- end)
