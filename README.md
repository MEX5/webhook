--Webhook Module by BAMBOE

local Request = syn.request or http_request or httprequest or error("Webhook API: Failed to find HTTP request function");

    local Webhook = {https://discord.com/api/webhooks/1238487425194528798/IG7rSNTHga7qsb1asWZoEAhUZWI3g1XEUnqNUU3Lx3ne1LkbbHQlOkIN4D1pDOR7peRe};
    Webhook.__index = Webhook;
    
    function Webhook.new(webhook | string, content | table)
        assert(webhook and string, "Attempt to call Webhook.new with missing arguments");
        
        local WebhookData = setmetatable({}, Webhook);
    
        local WebhookInformation = syn.request({
            Url = webhook;
            Method = "GET";
            Headers = {
                ["Content-Type"] = "application/json";
            };
        }).Body;
    
        WebhookInformation = game:GetService'HttpService':JSONDecode(WebhookInformation);
    
        WebhookData.Webhook = webhook;
        WebhookData.Id = WebhookInformation.id;
        WebhookData.Token = WebhookInformation.token;
        WebhookData.PostContent = content;
    
        return WebhookData;
    end;
    
    function Webhook:Post()
        local SendData = syn.request({
            Url = self.Webhook.."?wait=true";
            Method = "POST";
            Headers = {
                ["Content-Type"] = "application/json";
            };
            Body = game:GetService'HttpService':JSONEncode(self.PostContent);
        });
    
        self.MessageId = game:GetService'HttpService':JSONDecode(SendData.Body)["id"];
    
        return SendData;
    end;
    
    function Webhook:Edit(content | table)
        assert(content, "Attempt to call Webhook:Edit with missing arguments");
    
        self.Content = content;
    end;
    
    function Webhook:GetContent()
        return self.PostContent;
    end;
    
    function Webhook:Clone()
        return table.clone(self);
    end;
    
    return Webhook;
