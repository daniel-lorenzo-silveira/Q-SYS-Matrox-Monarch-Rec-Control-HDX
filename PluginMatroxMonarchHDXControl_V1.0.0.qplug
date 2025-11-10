--************************************************
--* Agradecimentos e Créditos
--************************************************
-- Este plugin foi desenvolvido por Daniel Lorenzo Silveira Alves (Estagiário)
-- Idealização do Projeto: Bruno Mariani de Melo
-- Colaboração e Suporte Técnico: Equipe SAVID do STJ

--********************************************************
-- 1. ESTRUTURA E METADADOS DO PLUGIN (Oficial)
--********************************************************

PluginInfo = { 
    Name = "User Matrox Monarch Control",
    Version = "1.0.0", 
    Id = "devbydaniellorenzosavid.stj.matroxmonarchcontrol", 
    DisplayName = "Matrox Dual Encoder Control",
    Author = "Daniel Lorenzo Silveira Alves daniel.lorenzo0610@gmail.com",
    Description = "Plugin desenvolvido para o controle de gravação e monitoramento dos dois encoders do dispositivo Matrox Monarch HDX",
    Manufacturer = "Matrox",
    Model = "Monarch HDX"   
}

-- Define a cor do plugin object no design
function GetColor(props)
    return { 102, 102, 102 }
end

-- O nome que será exibido inicialmente
function GetPrettyName(props)
    return "Matrox Dual Encoder Control v" .. PluginInfo.Version
end

-- Define as configurações de propriedades do plugin
function GetProperties()
    local props = {}
    table.insert(props, {Name = "HostIP", Type = "string", Value = "172.16.16.46", Display = "IP do Matrox", Group = "Configuração"})
    table.insert(props, {Name = "MatroxPort", Type = "string", Value = "443", Display = "Porta HTTPS", Group = "Configuração"})
    table.insert(props, {Name = "Username", Type = "string", Value = "admin", Display = "Usuário", Group = "Configuração"})
    table.insert(props, {Name = "Password", Type = "string", Value = "Savid@Gravacao", Display = "Senha", Group = "Configuração"})
    table.insert(props, {Name = "PollingInterval", Type = "integer", Min = 0 , Max = 30, Value = 10 ,Display = "Intervalo de Polling (s)", Group = "Polling"})
    table.insert(props, {Name = "MaxRetries", Type = "integer", Min = 0 , Max = 10, Value = 4 , Display = "Máximo de Tentativas", Group = "Polling"})
    return props
end 

-- Define os Controls usados no plugin
function GetControls(props)
    local ctrls = {}
    
    -- ENCODER 1 (GRAVAÇÃO)
    table.insert(ctrls, {
        Name = "ENC1_StartButton",
        ControlType = "Button",
        ButtonType = "Toggle", -- Usamos Toggle para manter o estado (Rec/Stop)
        Display = "Ação ENC1 (Start/Stop)",
        UserPin = true,
        PinStyle = "Input",
        Icon = "Record"
    })
    table.insert(ctrls, {Name = "ENC1_Mode", ControlType = "Text", Display = "Status ENC1: MODO"})
    table.insert(ctrls, {Name = "ENC1_State", ControlType = "Text", Display = "Status ENC1: ESTADO"})
    table.insert(ctrls, {Name = "ENC1_HTTPStatus", ControlType = "Text", Display = "Status ENC1: HTTP"})

    -- ENCODER 2
    table.insert(ctrls, {
        Name = "ENC2_StartButton",
        ControlType = "Button",
        ButtonType = "Toggle",
        Display = "Ação ENC2 (Start/Stop)",
        UserPin = true,
        PinStyle = "Input",
        Icon = "Record"
    })
    table.insert(ctrls, {Name = "ENC2_Mode" , ControlType = "Text", Display = "Status ENC2: MODO"})
    table.insert(ctrls, {Name = "ENC2_State", ControlType = "Text", Display = "Status ENC2: ESTADO"})
    table.insert(ctrls, {Name = "ENC2_HTTPStatus", ControlType = "Text", Display = "Status ENC2: HTTP"})
    
    return ctrls
end






-- Leiaute de controles e gráficos para a interface de usuário exibido do plugin
function GetControlLayout(props)
    local layout = {}
    local graphics = {}
    
    local START_X = 10
    local START_Y = 10
    local SPACING = 25
    local GROUP_WIDTH = 280
    local GROUP_HEIGHT = 150
    local X_LABEL = START_X + 5
    local X_VALUE = START_X + 110
    local X_BUTTON = START_X + 220
    
    -- GRÁFICO: BOX para ENCODER 1
    table.insert(graphics, {
        Type = "GroupBox",
        Text = "ENCODER 1",
        Fill = {50, 50, 50}, -- Cor de fundo escura
        StrokeWidth = 1,
        Position = {START_X, START_Y},
        Size = {GROUP_WIDTH, GROUP_HEIGHT}
    })
    
    -- GRÁFICO: BOX para ENCODER 2
    table.insert(graphics, {
        Type = "GroupBox",
        Text = "ENCODER 2",
        Fill = {50, 50, 50},
        StrokeWidth = 1,
        Position = {START_X, START_Y + GROUP_HEIGHT + 10},
        Size = {GROUP_WIDTH, GROUP_HEIGHT}
    })

    ---------------------------------------------------------
    -- CONTROLES DO ENCODER 1 (GRAVAÇÃO)
    ---------------------------------------------------------
    local Y1 = START_Y + 25

    -- Botão START/STOP
    layout["ENC1_StartButton"] = {
        PrettyName = "BOTAO INICIAR/PARAR",
        Style = "Button",
        Position = {X_BUTTON, Y1},
        Size = {60, 20},
        Color = {160, 82, 45} -- Cor inicial (IDLE)
    }

    -- Status MODO (LABEL)
    table.insert(graphics, { Type = "Label", Text = "MODO:", Position = {X_LABEL, Y1}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status MODO (VALOR)
    layout["ENC1_Mode"] = { PrettyName = "Status ENCODER 1 - MODO", Style = "Text", Position = {X_VALUE, Y1}, Size = {100, 20}, Color = {255, 255, 255} }
    Y1 = Y1 + SPACING

    -- Status ESTADO (LABEL)
    table.insert(graphics, { Type = "Label", Text = "ESTADO:", Position = {X_LABEL, Y1}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status ESTADO (VALOR)
    layout["ENC1_State"] = { PrettyName = "Status ENCODER 1 - ESTADO", Style = "Text", Position = {X_VALUE, Y1}, Size = {100, 20}, Color = {255, 255, 255} }
    Y1 = Y1 + SPACING
    
    -- Status HTTP (LABEL)
    table.insert(graphics, { Type = "Label", Text = "HTTP STATUS:", Position = {X_LABEL, Y1}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status HTTP (VALOR)
    layout["ENC1_HTTPStatus"] = {PrettyName = "Status ENCODER 1 - HTTP", Style = "Text", Position = {X_VALUE, Y1}, Size = {100, 20}, Color = {100, 200, 100} }


    -------------------------------------------------------
    -- CONTROLES DO ENCODER 2
    ------------------------------------------------------
    local Y2 = START_Y + GROUP_HEIGHT + 10 + 25
    
    -- Botão START/STOP
    layout["ENC2_StartButton"] = {
        PrettyName = "BOTAO INICIAR/PARAR",
        Style = "Button",
        Position = {X_BUTTON, Y2},
        Size = {60, 20},
        Color = {160, 82, 45}
    }

    -- Status MODO (LABEL)
    table.insert(graphics, { Type = "Text", Text = "MODO:", Position = {X_LABEL, Y2}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status MODO (VALOR)
    layout["ENC2_Mode"] = { PrettyName = "Status ENCODER 2 - MODO", Style = "Text", Position = {X_VALUE, Y2}, Size = {100, 20}, Color = {255, 255, 255} }
    Y2 = Y2 + SPACING

    -- Status ESTADO (LABEL)
    table.insert(graphics, { Type = "Text", Text = "ESTADO:", Position = {X_LABEL, Y2}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status ESTADO (VALOR)
    layout["ENC2_State"] = { PrettyName = "Status ENCODER 2 - ESTADO", Style = "Text", Position = {X_VALUE, Y2}, Size = {100, 20}, Color = {255, 255, 255} }
    Y2 = Y2 + SPACING
    
    -- Status HTTP (LABEL)
    table.insert(graphics, { Type = "Text", Text = "HTTP STATUS:", Position = {X_LABEL, Y2}, Size = {100, 20}, FontSize = 14, HTextAlign = "Right", Color = {255, 255, 255} })
    -- Status HTTP (VALOR)
    layout["ENC2_HTTPStatus"] = { PrettyName = "Status ENCODER 2 - HTTP", Style = "Text", Position = {X_VALUE, Y2}, Size = {100, 20}, Color = {100, 200, 100} }

    return layout, graphics
end

if Controls then
    --========================================================
    -- 1. PROPRIEDADES DE CONFIGURAÇÃO (Ajustado)
    --========================================================
    local HOST_IP = Properties["HostIP"].Value
    local MATROX_PORT = Properties["MatroxPort"].Value        
    local USERNAME = Properties["Username"].Value     
    local PASSWORD = Properties["Password"].Value
    --========================================================
    -- 2. VARIÁVEIS DE ESTADO E CONTROLES
    --========================================================
    local isRecording1    -- Estado do Encoder 1
    local isRecording2    -- Estado do Encoder 2 (NOVO)
    local json = require("json")

    -- VARIÁVEIS GLOBAIS DE PISCA (Duplicadas)
    local blinkingTimer1 = nil
    local blinkingTimer2 = nil -- NOVO
    local BLINK_INTERVAL = 0.5

    -- Cores
    local COLOR_IDLE_READY = "#A0522D"
    local COLOR_REC_LIGHT = "#FF4500"
    local COLOR_REC_DARK = "#8B0000"

    -- VARIÁVEL GLOBAL PARA PASSAR CONTEXTO DE COMANDO
    _G.CurrentCommandType = "STATUS" 
    _G.RetryCount = 0
    local MAX_RETRIES = Properties["MaxRetries"].Value
    -- CONTROLES (Duplicados)
    local StartButton1 = Controls.ENC1_StartButton
    local TextFieldStatus1 = {Controls.ENC1_Mode, Controls.ENC1_State, Controls.ENC1_HTTPStatus} -- Assume que é uma tabela de 3 campos [Mode, State, HTTP]
    
    local StartButton2 = Controls.ENC2_StartButton      -- NOVO BOTÃO
    local TextFieldStatus2 = {Controls.ENC2_Mode, Controls.ENC2_State, Controls.ENC2_HTTPStatus}  -- NOVO INDICADOR

    --========================================================
    -- 3. FUNÇÕES AUXILIARES
    --========================================================

    -- Funções de controle da pisca para ENC1
    local function stopBlinking1()
    if blinkingTimer1 then
        Timer.Clear(blinkingTimer1)
        blinkingTimer1 = nil
    end
    end

    local function startBlinking1()
    if not isRecording1 or not StartButton1 then
        stopBlinking1()
        if StartButton1 then
        StartButton1.Color = COLOR_IDLE_READY
        end
        return
    end
    if StartButton1.Color == COLOR_REC_LIGHT then
        StartButton1.Color = COLOR_REC_DARK
    else
        StartButton1.Color = COLOR_REC_LIGHT
    end
    blinkingTimer1 = Timer.CallAfter(startBlinking1, BLINK_INTERVAL)
    end

    -- Funções de controle da pisca para ENC2 (Duplicadas)
    local function stopBlinking2()
    if blinkingTimer2 then
        Timer.Clear(blinkingTimer2)
        blinkingTimer2 = nil
    end
    end

    local function startBlinking2()
    if not isRecording2 or not StartButton2 then
        stopBlinking2()
        if StartButton2 then
        StartButton2.Color = COLOR_IDLE_READY
        end
        return
    end
    if StartButton2.Color == COLOR_REC_LIGHT then
        StartButton2.Color = COLOR_REC_DARK
    else
        StartButton2.Color = COLOR_REC_LIGHT
    end
    blinkingTimer2 = Timer.CallAfter(startBlinking2, BLINK_INTERVAL)
    end


    -- Transforma os dados de status do encoder em uma tabela (MANTIDA)
    function split_status(status_string)
        local results = {}
        for chunk in string.gmatch(status_string, '([^,]+)') do
            local clean_chunk = string.gsub(chunk, "^%s*(.-)%s*$", "%1")
            table.insert(results, clean_chunk)
        end
        return results
    end

    -- Codificar estilos de ícone (MANTIDA)
    local function getIconStyle(iconCode)
    return json.encode{
        IconString = iconCode
    }
    end

    local ICON_STOP = getIconStyle(string.char(0xef,0x87,0xaf))
    local ICON_RECORD = getIconStyle(string.char(0xef,0x86,0xa4))
    local ICON_REFRESH = getIconStyle(string.char(0xef,0x86,0xa5))
    local ICON_ALERT = getIconStyle(string.char(0xef,0x84,0x81))

    -- Funções de atualização unificadas
    local function updateEncoderStatus(enc_num, enc_mode, enc_state, wasRecording, startButton, statusFields, stopBlinkFunc, startBlinkFunc)
        
        local isRecordingCurrent = (enc_mode == "RECORD" and enc_state == "ON")
        
        if enc_num == 1 then
            isRecording1 = isRecordingCurrent
        else
            isRecording2 = isRecordingCurrent
        end

        local mode_display = enc_mode or "N/A"
        local state_display = enc_state or "N/A"
        
        if isRecordingCurrent then
            if not wasRecording then
                stopBlinkFunc() 
                startButton.Color = COLOR_REC_LIGHT
                startBlinkFunc()
            end
            startButton.Style = ICON_STOP
            startButton.String = "STOP"
            statusFields[2].Color = COLOR_REC_LIGHT
        else
            if wasRecording then
                stopBlinkFunc()
                startButton.Color = COLOR_IDLE_READY
            else
                startButton.Color = COLOR_IDLE_READY
            end
            startButton.Style = ICON_RECORD
            startButton.String = "REC"
            statusFields[2].Color = COLOR_IDLE_READY -- Usa a cor de pronto/parado
        end

        statusFields[1].String = mode_display 
        statusFields[2].String = state_display 
    end


    function handle_response(tbl, code, data, err)
        local responseText = data
        local command_type = _G.CurrentCommandType 
        local isSuccessfulResponse = false
        
        -- TRATAMENTO DE CONECTIVIDADE (APLICA-SE AOS DOIS ENCODERS)
        if code == 200 and responseText ~= "RETRY" and responseText ~= "" then
            isSuccessfulResponse = true
            _G.RetryCount = 0
            TextFieldStatus1[3].String = "OK" -- HTTP Status para ENC1
            TextFieldStatus2[3].String = "OK" -- NOVO: HTTP Status para ENC2
        else
            _G.RetryCount = _G.RetryCount + 1
            Log.Error(string.format("Falha/Retry Matrox. Tentativas: %i. Code=%i. Erro=%s", _G.RetryCount, code, err or responseText))
            local retry_msg = string.format("RETRY... (%i/%i)", _G.RetryCount, MAX_RETRIES)
            TextFieldStatus1[3].String = retry_msg
            TextFieldStatus2[3].String = retry_msg -- NOVO
        end

        -- VERIFICAÇÃO DE TIMEOUT
        if _G.RetryCount >= MAX_RETRIES then
            local msg = string.format("TIMEOUT! Sem resposta há %d seg.", MAX_RETRIES * 2)
            TextFieldStatus1[3].String = msg
            TextFieldStatus2[3].String = msg -- NOVO
            
            -- DESLIGA TODOS OS CONTROLES VISUAIS
            local default_style = ICON_ALERT
            local default_color = "#808080"
            local default_text = "Desconhecido"
            
            StartButton1.Color = default_color
            StartButton1.Style = default_style
            TextFieldStatus1[1].String = default_text
            TextFieldStatus1[2].String = default_text
            stopBlinking1() -- Para a pisca do ENC1
            
            StartButton2.Color = default_color
            StartButton2.Style = default_style
            TextFieldStatus2[1].String = default_text
            TextFieldStatus2[2].String = default_text
            stopBlinking2() -- Para a pisca do ENC2
            return
        end
        
        -- CASO A REQ. HTTP FALHE/RETRY
        if not isSuccessfulResponse then
            StartButton1.Color = "#FFC000"
            StartButton1.Style = ICON_REFRESH
            stopBlinking1()
            
            StartButton2.Color = "#FFC000"
            StartButton2.Style = ICON_REFRESH
            stopBlinking2()
            return
        end

        -- LÓGICA DE CONTROLE (Mantida, pois só precisa forçar o status)
        if command_type == "CONTROL" then
            if string.find(responseText, "SUCCESS", 1, true) then
                Log.Message("Comando de controle Matrox OK. Forçando atualização de status.")
                sendMatroxRequest("GetStatus", "STATUS")
            else
                Log.Error("Comando Matrox falhou. Resposta: " .. responseText)
                TextFieldStatus1[3].String = "ERRO (Ver Log)"
                TextFieldStatus2[3].String = "ERRO (Ver Log)"
            end
            
    -- LÓGICA DE STATUS (Atualiza ENC1 e ENC2)
        elseif command_type == "STATUS" then
            
            -- Extrai o status do ENC1
            local enc1_status_full = string.match(responseText, "ENC1:(.-),ENC2:")
            if not enc1_status_full then enc1_status_full = string.match(responseText, "ENC1:(.*)") end
            local enc1_array = split_status(string.gsub(enc1_status_full or "", "%s", ""))
            
            -- Extrai o status do ENC2
            local enc2_status_full = string.match(responseText, "ENC2:(.-),NAME:") -- Assume que NAME: é o próximo campo
            if not enc2_status_full then enc2_status_full = string.match(responseText, "ENC2:(.*)") end
            local enc2_array = split_status(string.gsub(enc2_status_full or "", "%s", ""))

            -- ATUALIZA ENC1
            updateEncoderStatus(1, enc1_array[1], enc1_array[2], isRecording1, StartButton1, TextFieldStatus1, stopBlinking1, startBlinking1)
            
            -- ATUALIZA ENC2
            updateEncoderStatus(2, enc2_array[1], enc2_array[2], isRecording2, StartButton2, TextFieldStatus2, stopBlinking2, startBlinking2)
        end
    end

    --========================================================
    -- 5. FUNÇÃO GENÉRICA DE REQUISIÇÃO
    --========================================================
    function sendMatroxRequest(command, command_type)
        _G.CurrentCommandType = command_type
        local encoded_password = string.gsub(PASSWORD, "@", "%%40")
	local HOST_IP = Properties["HostIP"].Value 

        local URL_comando = string.format("https://%s:%s@%s:%d/Monarch/syncconnect/sdk.aspx?command=%s", 
                                        USERNAME, encoded_password, HOST_IP, MATROX_PORT, command)
        
        HttpClient.Get { 
            Url = URL_comando, 
            Timeout = 5,
            EventHandler = handle_response 
        }
    end

    --==============================================
    -- 6. CALLBACKS DOS BOTÕES
    --==============================================

    -- CALLBACK ENC1
    StartButton1.EventHandler = function()
        StartButton1.Color = "#8000FF" -- Feedback visual de clique
        if isRecording1 then
            sendMatroxRequest("StopEncoder1", "CONTROL")
        else
            sendMatroxRequest("StartEncoder1", "CONTROL")
        end
        StartButton1.Value = false
    end

    -- CALLBACK ENC2 (NOVO)
    StartButton2.EventHandler = function()
        StartButton2.Color = "#8000FF" -- Feedback visual de clique
        if isRecording2 then
            sendMatroxRequest("StopEncoder2", "CONTROL")
        else
            sendMatroxRequest("StartEncoder2", "CONTROL")
        end
        StartButton2.Value = false
    end



    --====================================================
    -- 7. CÓDIGO PRINCIPAL / INICIALIZAÇÃO
    --====================================================

    Log.Message("Script Matrox HDX v4.0 carregado. Iniciando Polling.")

    -- ATUALIZAÇÃO EM TEMPO REAL: Polling a cada PollingInterval segundos
    local function startPolling()
	PollingInterval = Properties["PollingInterval"].Value
    	sendMatroxRequest("GetStatus", "STATUS")
    	print(string.format("PollingInterval dentro da função start pooling %s", PollingInterval))
    	Timer.CallAfter(startPolling , PollingInterval)
    end

    startPolling()
    Log.Message(string.format("Inicialização concluída. Polling ativo a cada %s segundos.", Properties["PollingInterval"].Value))

end 