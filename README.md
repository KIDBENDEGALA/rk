-- Rkzin Hub: Loader Oficial
-- Salve este código num Pastefy e use o link RAW no seu executor.

local urlLocopani = "https://raw.githubusercontent.com/locopanison-crypto/Locopanix/refs/heads/main/LOCOPANI_RAYFIELD_GEN2%20(3).lua"
local urlCH = "https://pastefy.app/OxmnhmJY/raw"

-- 1. Bypass Locopani
local env = getgenv()
local oldReq = env.request or env.http_request or (syn and syn.request) or (http and http.request)
if oldReq then
    env.request = function(opts)
        local res = oldReq(opts)
        if opts.Url and string.find(opts.Url, "locopani%-activate") then
            if type(res) == "table" then
                res.Body = '{"status":"ACTIVATED","authorized":true}'
                res.StatusCode = 200
            end
        end
        return res
    end
    env.http_request = env.request
end

-- 2. Carrega o Locopani (Bypass + Rebrand)
local success, src = pcall(function() return game:HttpGet(urlLocopani) end)
if success then
    local bypassed = string.gsub(src, "local function LOCOPANI_ActivateKey%(key%)", "local function LOCOPANI_ActivateKey(key) return true, 'Bypass' end local _dummy_ActivateKey = function(key)")
    
    -- Troca o nome no código-fonte, mas protege a URL do Supabase
    bypassed = string.gsub(bypassed, "hshqmleifwkvzjrefabj", "PROTECTED_SUPABASE_URL")
    bypassed = string.gsub(bypassed, "Locopani", "Rkzin")
    bypassed = string.gsub(bypassed, "LOCOPANI", "RKZIN")
    bypassed = string.gsub(bypassed, "PROTECTED_SUPABASE_URL", "hshqmleifwkvzjrefabj")
    
    local func, err = loadstring(bypassed)
    if func then pcall(func) else warn("[Rkzin] Erro Locopani: " .. tostring(err)) end
end

-- 3. Hook Instantâneo (Troca o nome do CH HUB na tela)
local CoreGui = game:GetService("CoreGui")
local PlayerGui = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

local function fixText(obj)
    if obj:IsA("TextLabel") or obj:IsA("TextButton") or obj:IsA("TextBox") then
        if obj.Text then
            local txt = obj.Text
            if string.find(txt, "CH HUB") or string.find(txt, "CH Hub") or string.find(txt, "Ch Hub") or string.find(txt, "CH") then
                obj.Text = string.gsub(txt, "[Cc][Hh] [Hh][Uu][Bb]", "Rkzin")
                obj.Text = string.gsub(obj.Text, "[Cc][Hh][Hh][Uu][Bb]", "Rkzin")
                obj.Text = string.gsub(obj.Text, "[Cc][Hh]", "Rkzin")
            end
        end
    end
end

local function onDescendantAdded(obj)
    fixText(obj)
    if obj:IsA("TextLabel") or obj:IsA("TextButton") then
        obj:GetPropertyChangedSignal("Text"):Connect(function()
            fixText(obj)
        end)
    end
end

CoreGui.DescendantAdded:Connect(onDescendantAdded)
PlayerGui.DescendantAdded:Connect(onDescendantAdded)

for _, v in pairs(CoreGui:GetDescendants()) do onDescendantAdded(v) end
for _, v in pairs(PlayerGui:GetDescendants()) do onDescendantAdded(v) end

-- 4. Carrega o CH HUB
task.spawn(function()
    task.wait(1.5) 
    loadstring(game:HttpGet(urlCH))()
end)
