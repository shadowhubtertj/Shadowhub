-- ==========================================================
-- ⚔️ SHADOW HUB - SCRIPT APENAS DA INTERFACE (REDZ STYLE)
-- Save isso no GitHub/Pastebin para gerar o loadstring!
-- ==========================================================

local ShadowHub = {}

function ShadowHub:CreateMenu(hubName)
    hubName = hubName or "Shadow Hub ⚔️"
    
    local TweenService = game:GetService("TweenService")
    local UserInputService = game:GetService("UserInputService")
    
    local ScreenGui = Instance.new("ScreenGui")
    local MainFrame = Instance.new("Frame")
    local MainCorner = Instance.new("UICorner")
    local Sidebar = Instance.new("Frame")
    local SidebarCorner = Instance.new("UICorner")
    local SidebarList = Instance.new("UIListLayout")
    local SidebarPadding = Instance.new("UIPadding")
    local Topbar = Instance.new("Frame")
    local TopbarCorner = Instance.new("UICorner")
    local Title = Instance.new("TextLabel")
    local CloseBtn = Instance.new("ImageButton")
    local Container = Instance.new("Frame")
    local ContainerCorner = Instance.new("UICorner")
    local PagesFolder = Instance.new("Folder")

    -- Botão flutuante (Toggle)
    local ToggleButton = Instance.new("TextButton")
    local ToggleCorner = Instance.new("UICorner")
    local ToggleStroke = Instance.new("UIStroke")

    local success, _ = pcall(function()
        ScreenGui.Parent = game:GetService("CoreGui")
    end)
    if not success then
        ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    end
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    -- Painel Principal
    MainFrame.Name = "ShadowMain"
    MainFrame.Parent = ScreenGui
    MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    MainFrame.Position = UDim2.new(0.25, 0, 0.25, 0)
    MainFrame.Size = UDim2.new(0, 540, 0, 320)
    MainFrame.ClipsDescendants = true
    MainCorner.CornerRadius = UDim.new(0, 9)
    MainCorner.Parent = MainFrame

    -- Botão Flutuante
    ToggleButton.Name = "ShadowToggleButton"
    ToggleButton.Parent = ScreenGui
    ToggleButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    ToggleButton.Position = UDim2.new(0.1, 0, 0.15, 0)
    ToggleButton.Size = UDim2.new(0, 45, 0, 45)
    ToggleButton.Font = Enum.Font.GothamBold
    ToggleButton.Text = "⚔️"
    ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleButton.TextSize = 18
    ToggleButton.ZIndex = 10

    ToggleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCorner.Parent = ToggleButton

    ToggleStroke.Color = Color3.fromRGB(255, 50, 70)
    ToggleStroke.Thickness = 1.5
    ToggleStroke.Parent = ToggleButton

    -- Arrastar Botão
    local btnDragging, btnDragStart, btnStartPos
    ToggleButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            btnDragging = true btnDragStart = input.Position btnStartPos = ToggleButton.Position
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if btnDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local Delta = input.Position - btnDragStart
            ToggleButton.Position = UDim2.new(btnStartPos.X.Scale, btnStartPos.X.Offset + Delta.X, btnStartPos.Y.Scale, btnStartPos.Y.Offset + Delta.Y)
        end
    end)
    ToggleButton.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then btnDragging = false end
    end)

    -- Abrir / Fechar
    local menuVisible = true
    ToggleButton.MouseButton1Click:Connect(function()
        menuVisible = not menuVisible
        MainFrame.Visible = menuVisible
    end)

    -- Arrastar Painel Principal
    local Dragging, DragInput, DragStart, StartPosition
    Topbar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            Dragging = true DragStart = input.Position StartPosition = MainFrame.Position
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if Dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local Delta = input.Position - DragStart
            MainFrame.Position = UDim2.new(StartPosition.X.Scale, StartPosition.X.Offset + Delta.X, StartPosition.Y.Scale, StartPosition.Y.Offset + Delta.Y)
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then Dragging = false end
    end)

    -- Topbar
    Topbar.Name = "Topbar"
    Topbar.Parent = MainFrame
    Topbar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    Topbar.Size = UDim2.new(1, 0, 0, 35)
    TopbarCorner.CornerRadius = UDim.new(0, 9)
    TopbarCorner.Parent = Topbar

    Title.Parent = Topbar
    Title.BackgroundTransparency = 1
    Title.Position = UDim2.new(0, 15, 0, 0)
    Title.Size = UDim2.new(0, 200, 1, 0)
    Title.Font = Enum.Font.GothamBold
    Title.Text = hubName
    Title.TextColor3 = Color3.fromRGB(255, 50, 70)
    Title.TextSize = 15
    Title.TextXAlignment = Enum.TextXAlignment.Left

    CloseBtn.Parent = Topbar
    CloseBtn.BackgroundTransparency = 1
    CloseBtn.Position = UDim2.new(1, -30, 0, 5)
    CloseBtn.Size = UDim2.new(0, 25, 0, 25)
    CloseBtn.Image = "rbxassetid://6031094678"
    CloseBtn.ImageColor3 = Color3.fromRGB(255, 255, 255)
    CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

    -- Sidebar
    Sidebar.Name = "Sidebar"
    Sidebar.Parent = MainFrame
    Sidebar.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
    Sidebar.Position = UDim2.new(0, 5, 0, 40)
    Sidebar.Size = UDim2.new(0, 130, 1, -45)
    SidebarCorner.CornerRadius = UDim.new(0, 8)
    SidebarCorner.Parent = Sidebar

    local SidebarScroll = Instance.new("ScrollingFrame")
    SidebarScroll.Parent = Sidebar
    SidebarScroll.Size = UDim2.new(1, 0, 1, 0)
    SidebarScroll.BackgroundTransparency = 1
    SidebarScroll.BorderSizePixel = 0
    SidebarScroll.ScrollBarThickness = 2
    SidebarScroll.ScrollBarImageColor3 = Color3.fromRGB(255, 50, 70)

    SidebarList.Parent = SidebarScroll
    SidebarList.SortOrder = Enum.SortOrder.LayoutOrder
    SidebarList.Padding = UDim.new(0, 5)
    SidebarPadding.Parent = SidebarScroll
    SidebarPadding.PaddingTop = UDim.new(0, 5)
    SidebarPadding.PaddingLeft = UDim.new(0, 5)
    SidebarPadding.PaddingRight = UDim.new(0, 5)

    SidebarList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        SidebarScroll.CanvasSize = UDim2.new(0, 0, 0, SidebarList.AbsoluteContentSize.Y + 10)
    end)

    -- Container
    Container.Name = "Container"
    Container.Parent = MainFrame
    Container.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
    Container.Position = UDim2.new(0, 140, 0, 40)
    Container.Size = UDim2.new(1, -145, 1, -45)
    ContainerCorner.CornerRadius = UDim.new(0, 8)
    ContainerCorner.Parent = Container
    PagesFolder.Parent = Container

    local Sections = {}
    local firstPage = true

    function Sections:CreateSection(tabName)
        local TabBtn = Instance.new("TextButton")
        local TabBtnCorner = Instance.new("UICorner")
        
        TabBtn.Parent = SidebarScroll
        TabBtn.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
        TabBtn.BackgroundTransparency = 1
        TabBtn.Size = UDim2.new(1, 0, 0, 30)
        TabBtn.Font = Enum.Font.GothamSemibold
        TabBtn.Text = tabName
        TabBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
        TabBtn.TextSize = 12
        TabBtnCorner.CornerRadius = UDim.new(0, 6)
        TabBtnCorner.Parent = TabBtn

        local Page = Instance.new("ScrollingFrame")
        local PageList = Instance.new("UIListLayout")
        local PagePadding = Instance.new("UIPadding")

        Page.Parent = PagesFolder
        Page.Size = UDim2.new(1, 0, 1, 0)
        Page.BackgroundTransparency = 1
        Page.BorderSizePixel = 0
        Page.ScrollBarThickness = 3
        Page.ScrollBarImageColor3 = Color3.fromRGB(255, 50, 70)
        Page.Visible = false
        PageList.Parent = Page
        PageList.HorizontalAlignment = Enum.HorizontalAlignment.Center
        PageList.SortOrder = Enum.SortOrder.LayoutOrder
        PageList.Padding = UDim.new(0, 6)
        PagePadding.Parent = Page
        PagePadding.PaddingTop = UDim.new(0, 8)

        local function UpdateCanvas()
            Page.CanvasSize = UDim2.new(0, 0, 0, PageList.AbsoluteContentSize.Y + 20)
        end
        PageList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(UpdateCanvas)

        TabBtn.MouseButton1Click:Connect(function()
            for _, v in next, PagesFolder:GetChildren() do v.Visible = false end
            for _, v in next, SidebarScroll:GetChildren() do
                if v:IsA("TextButton") then
                    TweenService:Create(v, TweenInfo.new(0.2), {BackgroundTransparency = 1, TextColor3 = Color3.fromRGB(150, 150, 150)}):Play()
                end
            end
            Page.Visible = true
            TweenService:Create(TabBtn, TweenInfo.new(0.2), {BackgroundTransparency = 0, BackgroundColor3 = Color3.fromRGB(255, 50, 70), TextColor3 = Color3.fromRGB(255, 255, 255)}):Play()
        end)

        if firstPage then
            Page.Visible = true
            TabBtn.BackgroundTransparency = 0
            TabBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 70)
            TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            firstPage = false
        end

        local Elements = {}

        function Elements:TextLabel(text)
            local LabelFrame = Instance.new("Frame")
            local LabelCorner = Instance.new("UICorner")
            local LabelText = Instance.new("TextLabel")

            LabelFrame.Parent = Page
            LabelFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            LabelFrame.Size = UDim2.new(0.95, 0, 0, 35)
            LabelCorner.CornerRadius = UDim.new(0, 6)
            LabelCorner.Parent = LabelFrame

            LabelText.Parent = LabelFrame
            LabelText.Size = UDim2.new(1, 0, 1, 0)
            LabelText.BackgroundTransparency = 1
            LabelText.Font = Enum.Font.GothamSemibold
            LabelText.Text = text
            LabelText.TextColor3 = Color3.fromRGB(200, 200, 200)
            LabelText.TextSize = 13
        end

        function Elements:TextButton(btnText, descText, callback)
            callback = callback or function() end
            local BtnFrame = Instance.new("Frame")
            local BtnCorner = Instance.new("UICorner")
            local ClickBtn = Instance.new("TextButton")
            local ClickCorner = Instance.new("UICorner")
            local DescLabel = Instance.new("TextLabel")

            BtnFrame.Parent = Page
            BtnFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            BtnFrame.Size = UDim2.new(0.95, 0, 0, 45)
            BtnCorner.CornerRadius = UDim.new(0, 6)
            BtnCorner.Parent = BtnFrame

            ClickBtn.Parent = BtnFrame
            ClickBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 70)
            ClickBtn.Position = UDim2.new(0.02, 0, 0.2, 0)
            ClickBtn.Size = UDim2.new(0, 120, 0, 26)
            ClickBtn.Font = Enum.Font.GothamBold
            ClickBtn.Text = btnText
            ClickBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            ClickBtn.TextSize = 12
            ClickCorner.CornerRadius = UDim.new(0, 5)
            ClickCorner.Parent = ClickBtn

            DescLabel.Parent = BtnFrame
            DescLabel.BackgroundTransparency = 1
            DescLabel.Position = UDim2.new(0.38, 0, 0, 0)
            DescLabel.Size = UDim2.new(0.6, 0, 1, 0)
            DescLabel.Font = Enum.Font.GothamSemibold
            DescLabel.Text = descText
            DescLabel.TextColor3 = Color3.fromRGB(140, 140, 140)
            DescLabel.TextSize = 12
            DescLabel.TextXAlignment = Enum.TextXAlignment.Right
            DescLabel.PaddingRight = UDim.new(0, 10)

            ClickBtn.MouseButton1Click:Connect(function()
                ClickBtn.BackgroundColor3 = Color3.fromRGB(200, 30, 50)
                task.wait(0.1)
                ClickBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 70)
                callback()
            end)
        end

        function Elements:Toggle(toggleText, callback)
            callback = callback or function() end
            local TogFrame = Instance.new("Frame")
            local TogCorner = Instance.new("UICorner")
            local TogTitle = Instance.new("TextLabel")
            local Switch = Instance.new("TextButton")
            local SwitchCorner = Instance.new("UICorner")

            TogFrame.Parent = Page
            TogFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            TogFrame.Size = UDim2.new(0.95, 0, 0, 42)
            TogCorner.CornerRadius = UDim.new(0, 6)
            TogCorner.Parent = TogFrame

            TogTitle.Parent = TogFrame
            TogTitle.BackgroundTransparency = 1
            TogTitle.Position = UDim2.new(0.15, 0, 0, 0)
            TogTitle.Size = UDim2.new(0.8, 0, 1, 0)
            TogTitle.Font = Enum.Font.GothamSemibold
            TogTitle.Text = toggleText
            TogTitle.TextColor3 = Color3.fromRGB(220, 220, 220)
            TogTitle.TextSize = 13
            TogTitle.TextXAlignment = Enum.TextXAlignment.Left

            Switch.Parent = TogFrame
            Switch.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
            Switch.Position = UDim2.new(0.02, 0, 0.22, 0)
            Switch.Size = UDim2.new(0, 40, 0, 22)
            Switch.Text = ""
            SwitchCorner.CornerRadius = UDim.new(0, 11)
            SwitchCorner.Parent = Switch

            local Toggled = false
            Switch.MouseButton1Click:Connect(function()
                Toggled = not Toggled
                callback(Toggled)
                if Toggled then
                    game:GetService("TweenService"):Create(Switch, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(255, 50, 70)}):Play()
                else
                    game:GetService("TweenService"):Create(Switch, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play()
                end
            end)
        end

        return Elements
    end
    return Sections
end

-- IMPORTANTE: Retorna a biblioteca para que o loadstring consiga lê-la externa!
return ShadowHub

