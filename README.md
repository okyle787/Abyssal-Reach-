-- [[ 
--    Abyssal Reach - VenturaUI v3.3
--    Abyssal Eye Edition (Beta)
-- ]]

local Library = loadstring(game:HttpGet("https://codeberg.org/VenomVent/Ventura-UI/raw/branch/main/VenturaLibrary.lua"))()

-----------------------------------------------------------------------------
-- SERVIÇOS E PERSISTÊNCIA
-----------------------------------------------------------------------------
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")
local VIM = game:GetService("VirtualInputManager")

getgenv().AutoSniperEnabled = getgenv().AutoSniperEnabled or false
getgenv().SniperPrediction = getgenv().SniperPrediction or 3
getgenv().SpeedEnabled = getgenv().SpeedEnabled or false
getgenv().WalkSpeedValue = getgenv().WalkSpeedValue or 16
getgenv().CurrentLangName = getgenv().CurrentLangName or "Portuguese"

local Localization = {
    ["English"] = {main = "Main", shot = "Sniper Combat", tp = "Teleport", auto = "Auto Sniper", pred = "Prediction", mov = "Movement", speed = "Speed", val = "Value", eye = "Abyssal Eye 👁️", betaTitle = "Beta Version", betaDesc = "This is a beta version. Bugs may occur. If you find any issues, please report them on our Discord.", copyDesc = "Copy Discord Link", lang = "Languages"},
    ["Spanish"] = {main = "Principal", shot = "Francotirador", tp = "Teletransporte", auto = "Auto Sniper", pred = "Predicción", mov = "Movimiento", speed = "Velocidad", val = "Valor", eye = "Ojo Abisal 👁️", betaTitle = "Versión Beta", betaDesc = "Esta es una versión beta. Pueden ocurrir errores. Si encuentra problemas, infórmenos en nuestro Discord.", copyDesc = "Copiar Discord", lang = "Idiomas"},
    ["Portuguese"] = {main = "Principal", shot = "Combate Sniper", tp = "Teleportar", auto = "Auto Sniper", pred = "Previsão", mov = "Movimentação", speed = "Velocidade", val = "Valor", eye = "Olho Abissal 👁️", betaTitle = "Versão Beta", betaDesc = "Esta é uma versão beta. Erros podem ocorrer. Se encontrar problemas, relate em nosso Discord.", copyDesc = "Copiar Link do Discord", lang = "Idiomas"},
    ["Mandarin"] = {main = "主要", shot = "狙击战斗", tp = "传送", auto = "自动狙击", pred = "预测", mov = "移动", speed = "速度", val = "数值", eye = "深渊之眼 👁️", betaTitle = "测试版本", betaDesc = "这是测试版。可能会出现错误。如果发现问题，请在 Discord 上报告。", copyDesc = "复制 Discord 链接", lang = "语言"},
    ["Hindi"] = {main = "मुख्य", shot = "स्नाइपर", tp = "टेलीपोर्ट", auto = "ऑटो स्नाइपर", pred = "भविष्यवाणी", mov = "गति", speed = "गति", val = "मान", eye = "अगाध आँख 👁️", betaTitle = "बीटा संस्करण", betaDesc = "यह बीटा संस्करण है। त्रुटियां हो सकती हैं। समस्या होने पर डिस्कॉर्ड पर रिपोर्ट करें।", copyDesc = "डिस्कॉर्ड लिंक कॉपी करें", lang = "भाषाएं"},
    ["French"] = {main = "Principal", shot = "Combat", tp = "Téléport", auto = "Auto Sniper", pred = "Prédiction", mov = "Mouvement", speed = "Vitesse", val = "Valeur", eye = "Oeil Abyssal 👁️", betaTitle = "Version Bêta", betaDesc = "Ceci est une version bêta. Des bugs peuvent survenir. Signalez-les sur notre Discord.", copyDesc = "Copier le lien Discord", lang = "Langues"},
    ["Arabic"] = {main = "الرئيسية", shot = "قنص", tp = "انتقال", auto = "قنص تلقائي", pred = "توقع", mov = "حركة", speed = "سرعة", val = "قيمة", eye = "العين السحيقة 👁️", betaTitle = "نسخة بيتا", betaDesc = "هذه نسخة تجريبية. قد تحدث أخطاء. يرجى الإبلاغ عنها عبر ديسكورد.", copyDesc = "نسخ رابط ديسكورد", lang = "اللغات"},
    ["Bengali"] = {main = "প্রধান", shot = "স্নাইপার", tp = "টেলিপোর্ট", auto = "অটো স্নাইপার", pred = "পূর্বাভাস", mov = "চলাচল", speed = "গতি", val = "মান", eye = "অতল চোখ 👁️", betaTitle = "বিটা সংস্করণ", betaDesc = "এটি একটি বিটা সংস্করণ। ভুল হতে পারে। সমস্যা হলে ডিসকর্ডে জানান।", copyDesc = "ডিসকর্ড লিঙ্ক কপি করুন", lang = "ভাষা"},
    ["Russian"] = {main = "Главная", shot = "Снайпер", tp = "Телепорт", auto = "Авто-снайпер", pred = "Прогноз", mov = "Движение", speed = "Скорость", val = "Значение", eye = "Бездонное Око 👁️", betaTitle = "Бета-версия", betaDesc = "Это бета-версия. Возможны ошибки. Сообщайте о них в нашем Discord.", copyDesc = "Копировать Discord", lang = "Языки"},
    ["Japanese"] = {main = "メイン", shot = "狙撃", tp = "テレポート", auto = "自動狙撃", pred = "予測", mov = "移動", speed = "速度", val = "値", eye = "アビスの目 👁️", betaTitle = "ベータ版", betaDesc = "これはベータ版です。バグが発生する可能性があります。問題はDiscordで報告してください。", copyDesc = "Discordをコピー", lang = "言語"}
}

-----------------------------------------------------------------------------
-- CONSTRUTOR DA INTERFACE
-----------------------------------------------------------------------------
local function BuildUI()
    local Lang = Localization[getgenv().CurrentLangName]

    local GUI = Library:new({
        name        = "Abyssal Reach",
        subtitle    = {"Beta Engine", "discord.gg/bdFzJn9q4"},
        accent      = Color3.fromRGB(255, 255, 255),
        toggleKey   = Enum.KeyCode.LeftControl,
        minimizeKey = Enum.KeyCode.K,
        loadingTime = 1.2, 
        keyEnabled  = false,
    })

    task.spawn(function()
        task.wait(1.5)
        for _, v in pairs(CoreGui:GetChildren()) do
            if v:IsA("ScreenGui") and v:FindFirstChild("Main") and v.Main:FindFirstChild("Topbar") then
                getgenv().ActiveVentura = v
            end
        end
    end)

    -- [ ABA PRINCIPAL ]
    GUI:NavSection(Lang.main)
    local MainTab = GUI:CreateTab({ name = Lang.main, icon = "🎯" })

    MainTab:Section({ name = Lang.shot })
    MainTab:Button({ name = Lang.tp, callback = function()
        local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.CFrame = CFrame.new(-347.75061, 80.9382935, -133.011917) end
    end})
    MainTab:Toggle({ name = Lang.auto, default = getgenv().AutoSniperEnabled, callback = function(v) getgenv().AutoSniperEnabled = v end })
    MainTab:Slider({ name = Lang.pred, min = 0, max = 10, default = getgenv().SniperPrediction, callback = function(v) getgenv().SniperPrediction = v end })

    MainTab:Section({ name = Lang.mov })
    MainTab:Toggle({ name = Lang.speed, default = getgenv().SpeedEnabled, callback = function(v) getgenv().SpeedEnabled = v end })
    MainTab:Slider({ name = Lang.val, min = 16, max = 250, default = getgenv().WalkSpeedValue, callback = function(v) getgenv().WalkSpeedValue = v end })

    -- [ ABA ABYSSAL EYE (INFO BETA) ]
    GUI:NavSection("INFO")
    local InfoTab = GUI:CreateTab({ name = Lang.eye, icon = "👁️" })
    
    InfoTab:Section({ name = Lang.betaTitle })
    InfoTab:Label({ text = Lang.betaDesc })
    InfoTab:Chip({ text = "v0.3.3 BETA", color = Color3.fromRGB(255, 100, 100), icon = "⚠️" })
    
    InfoTab:Separator()
    
    InfoTab:Button({
        name = Lang.copyDesc,
        callback = function()
            setclipboard("https://discord.gg/bdFzJn9q4")
            GUI.notify("Abyssal Reach", "Link copied to clipboard!", 3)
        end
    })

    -- [ ABA DE LINGUAGENS ]
    GUI:NavSection("CONFIG")
    local LangTab = GUI:CreateTab({ name = Lang.lang, icon = "🌐" })

    for Name, _ in pairs(Localization) do
        LangTab:Button({ name = Name, callback = function()
            getgenv().CurrentLangName = Name
            if getgenv().ActiveVentura then getgenv().ActiveVentura:Destroy() end
            BuildUI()
        end})
    end
end

-----------------------------------------------------------------------------
-- BOTÃO MOBILE
-----------------------------------------------------------------------------
if not getgenv().MobileButtonSetup then
    local OpenUI = Instance.new("ScreenGui", CoreGui)
    OpenUI.Name = "AbyssalToggle"
    OpenUI.ResetOnSpawn = false
    
    local Button = Instance.new("ImageButton", OpenUI)
    Button.Size = UDim2.fromOffset(55, 55)
    Button.Position = UDim2.new(0.02, 0, 0.85, 0)
    Button.BackgroundColor3 = Color3.new(1, 1, 1)
    Button.BackgroundTransparency = 0.2
    Button.Image = "rbxassetid://123990318745407"
    Button.Draggable = true
    Instance.new("UICorner", Button).CornerRadius = UDim.new(0.2, 0)

    Button.MouseButton1Click:Connect(function()
        if getgenv().ActiveVentura and getgenv().ActiveVentura.Parent then
            VIM:SendKeyEvent(true, Enum.KeyCode.LeftControl, false, game)
            task.wait(0.01)
            VIM:SendKeyEvent(false, Enum.KeyCode.LeftControl, false, game)
        else
            BuildUI()
        end
    end)
    getgenv().MobileButtonSetup = true
end

-----------------------------------------------------------------------------
-- LOOPS (SNIPER & SPEED)
-----------------------------------------------------------------------------
if not getgenv().LoopsStarted then
    task.spawn(function()
        while task.wait(0.5) do
            if getgenv().AutoSniperEnabled then
                pcall(function()
                    local targets = {}
                    for _, p in ipairs(Players:GetPlayers()) do
                        if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                            table.insert(targets, p)
                        end
                    end
                    if #targets > 0 then
                        local target = targets[math.random(1, #targets)]
                        local pos = target.Character.HumanoidRootPart.Position + (target.Character.Humanoid.MoveDirection * getgenv().SniperPrediction)
                        game:GetService("ReplicatedStorage").Functions.RemoteFunctions.SniperShoot:InvokeServer(pos, Vector3.new(0, -1, 0), 0)
                    end
                end)
            end
        end
    end)

    task.spawn(function()
        game:GetService("RunService").Heartbeat:Connect(function()
            local h = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
            if h and getgenv().SpeedEnabled then
                h.WalkSpeed = getgenv().WalkSpeedValue
            elseif h and not getgenv().SpeedEnabled then
                h.WalkSpeed = 16
            end
        end)
    end)
    getgenv().LoopsStarted = true
end

if getgenv().ActiveVentura then getgenv().ActiveVentura:Destroy() end
BuildUI()
