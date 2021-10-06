local fruit_list = {
    ["flame"] = "Flame-Flame",
    ["dark"] = "Dark-Dark",
    ["quake"] = "Quake-Quake",
    ["light"] = "Light-Light",
    ["human"] = "Human-Human: Buddha",
    ["rubber"] = "Rubber-Rubber",
    ["spring"] = "Spring-Spring",
    ["kilo"] = "Kilo-Kilo",
    ["diamond"] = "Diamond-Diamond",
    ["love"] = "Love-Love",
    ["spin"] = "Spin-Spin",
    ["door"] = "Door-Door",
    ["ice"] = "Ice-Ice",
    ["control"] = "Control-Control",
    ["dragon"] = "Dragon-Dragon",
    ["magma"] = "Magma-Magma",
    ["venom"] = "Venom-Venom",
    ["sand"] = "Sand-Sand",
    ["bomb"] = "Bomb-Bomb",
    ["spike"] = "Spike-Spike",
    ["chop"] = "Chop-Chop",
    ["smoke"] = "Smoke-Smoke",
    ["phoenix"] = "Bird-Bird: Phoenix",
    ["falcon"] = "Bird-Bird: Falcon",
    ["rumble"] = "Rumble-Rumble",
    ["string"] = "String-String",
    ["gravity"] = "Gravity-Gravity",
    ["paw"] = "Paw-Paw",
    ["barrier"] = "Barrier-Barrier",
    ["dough"] = "Dough-Dough"
}
function find_fruit_id()
    for i,v in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
        if v:IsA("Tool") and v.Name:lower():find("fruit") then
            for i2,v2 in pairs(fruit_list) do
                if v.Name:lower():find(i2) then
                    return {v,v2}
                end
            end
        end
    end
    return nil
end
function store_fruit(fruit)
    local fruit_data = {}
    local arg1 = "getInventoryFruits"
    for i,v in pairs(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(arg1)) do
        table.insert(fruit_data,tostring(v['Name']))
    end
    for i,v in pairs(fruit_data) do
        if fruit[2] == v then
            print("Duplicate found")
            return
        end
    end
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", fruit[2])
end
function collect_fruit(tool)
    firetouchinterest(tool.Handle, game.Players.LocalPlayer.Character.PrimaryPart, 0)
    wait(1)
    local fruit = find_fruit_id()
    if fruit == nil then
        print("Fruit is invalid")
    else
        store_fruit(fruit)
    end
end
function server_hop()
    local PlaceID = game.PlaceId
    local AllIDs = {}
    local foundAnything = ""
    local actualHour = os.date("!*t").hour
    local Deleted = false
    local File = pcall(function()
        AllIDs = game:GetService('HttpService'):JSONDecode(readfile("NotSameServers.json"))
    end)
    if not File then
        table.insert(AllIDs, actualHour)
        writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
    end
    function TPReturner()
        local Site;
        if foundAnything == "" then
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
        else
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
        end
        local ID = ""
        if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
            foundAnything = Site.nextPageCursor
        end
        local num = 0;
        for i,v in pairs(Site.data) do
            local Possible = true
            ID = tostring(v.id)
            if tonumber(v.maxPlayers) > tonumber(v.playing) then
                for _,Existing in pairs(AllIDs) do
                    if num ~= 0 then
                        if ID == tostring(Existing) then
                            Possible = false
                        end
                    else
                        if tonumber(actualHour) ~= tonumber(Existing) then
                            local delFile = pcall(function()
                                delfile("NotSameServers.json")
                                AllIDs = {}
                                table.insert(AllIDs, actualHour)
                            end)
                        end
                    end
                    num = num + 1
                end
                if Possible == true then
                    table.insert(AllIDs, ID)
                    wait()
                    pcall(function()
                        writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                        wait()
                        game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                    end)
                    wait(2)
                end
            end
        end
    end
    
    function Teleport()
        while wait() do
            pcall(function()
                TPReturner()
                if foundAnything ~= "" then
                    TPReturner()
                end
            end)
        end
    end
    Teleport()
end

if not game:IsLoaded() then
    game.Loaded:Wait()
end

repeat wait() until game.Players.LocalPlayer.Character
repeat wait() until game.Players.LocalPlayer.Character.PrimaryPart

if game.Players.LocalPlayer.Team ~= "Pirates" then
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetTeam", "Pirates")
end

for i,v in pairs(game:GetService("Workspace"):GetChildren()) do
    if v:IsA("Tool") then
        print("Fruit Found")
        collect_fruit(v)
    end
end
server_hop()
