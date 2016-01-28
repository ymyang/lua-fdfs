# lua-fdfs
FastDFS LUA client

<pre>

local tracker = require('lib.fastdfs.tracker')
local storage = require('lib.fastdfs.storage')

local tk = tracker:new()
tk:set_timeout(3000)
local ok, err = tk:connect({host='192.168.85.249',port=22122})

function _dump_res(res)
    for i in pairs(res) do
        ngx.say(string.format("%s:%s",i, res[i]))
    end
    ngx.say("")
end

if not ok then
    ngx.say('connect error:' .. err)
    ngx.exit(200)
end

local res, err = tk:query_storage_store()
if not res then
    ngx.say("query storage error:" .. err)
    ngx.exit(200)
end

--[[
local res, err = tk:query_storage_update1("group1/M00/00/00/wKhV-VFbkMQEAAAAAAAAAKbO3LA494.txt")
if not res then
    ngx.say("query storage error:" .. err)
    ngx.exit(200)
end

local res, err = tk:query_storage_fetch1("group1/M00/00/00/wKhV-VFbkFfR1owfAAAAD6bO3LA348.txt")
if not res then
    ngx.say("query storage error:" .. err)
    ngx.exit(200)
end

]]

if not res then
    ngx.say("query storage error:" .. err)
    ngx.exit(200)
end



local st = storage:new()
st:set_timeout(3000)
local ok, err = st:connect(res)
if not ok then
    ngx.say("connect storage error:" .. err)
    ngx.exit(200)
end



local ok, err = st:delete_file1("group1/M00/00/00/wKhV-VFY71sEAAAAAAAAAKbO3LA277.txt")
if not ok then
    ngx.say("Fail:")
else
    ngx.say("OK")
end



local res, err = st:upload_by_buff('abcdedfg','txt')
if not res then
    ngx.say("upload error:" .. err)
    ngx.exit(200)
end


local res, err = st:upload_appender_by_buff('abcdedfg','txt')
if not res then
    ngx.say("upload error:" .. err)
    ngx.exit(200)
end


local ok, err = st:append_by_buff1("group1/M00/00/00/wKhV-VFbkMQEAAAAAAAAAKbO3LA494.txt","abcdedfg\n")
if not ok then
    ngx.say("Fail:")
else
    ngx.say("OK")
end


local res, err = tk:query_storage_update1("group1/M00/00/00/wKhV-VFbkFfR1owfAAAAD6bO3LA348.txt")
if not res then
    ngx.say("query storate error:" .. err)
    ngx.exit(200)
end




</pre>