<?xml version="1.0" encoding="UTF-8"?>
<bpmn:definitions xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL"
  xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI"
  xmlns:dc="http://www.omg.org/spec/DD/20100524/DC"
  xmlns:di="http://www.omg.org/spec/DD/20100524/DI"
  id="Definitions_Boletos" targetNamespace="http://bpmn.io/schema/bpmn">

  <bpmn:message id="Msg_Malote" name="Malote de boletos enviado"/>
  <bpmn:message id="Msg_CSV" name="Arquivo CSV gerado"/>
  <bpmn:message id="Msg_ErroImportacao" name="Erro na importação"/>

  <bpmn:collaboration id="Collab_Boletos">
    <bpmn:participant id="Part_Cliente" name="Cliente (remetente do malote)" processRef="Proc_Cliente"/>
    <bpmn:participant id="Part_Escritorio" name="Escritório de Contabilidade" processRef="Proc_Escritorio"/>
    <bpmn:participant id="Part_ERP" name="Sistema ERP Contábil" processRef="Proc_ERP"/>
    <bpmn:messageFlow id="MF_Malote" sourceRef="Task_Enviar" targetRef="Start_Malote"/>
    <bpmn:messageFlow id="MF_CSV" sourceRef="Task_Confirmar" targetRef="Start_ERP"/>
    <bpmn:messageFlow id="MF_Erro" sourceRef="End_Falha" targetRef="Task_VerificarErros"/>
  </bpmn:collaboration>

  <!-- PISCINA: CLIENTE -->
  <bpmn:process id="Proc_Cliente" isExecutable="false">
    <bpmn:startEvent id="Start_Cliente" name="Malote pronto para envio"/>
    <bpmn:task id="Task_Enviar" name="Enviar malote de boletos"/>
    <bpmn:sequenceFlow id="F_C1" sourceRef="Start_Cliente" targetRef="Task_Enviar"/>
  </bpmn:process>

  <!-- PISCINA: ESCRITÓRIO -->
  <bpmn:process id="Proc_Escritorio" isExecutable="false">
    <bpmn:laneSet id="LaneSet_Escritorio">
      <bpmn:lane id="Lane_Assistente" name="Assistente Contábil">
        <bpmn:flowNodeRef>Start_Malote</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Abrir</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_VerificarFalha</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Validar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gw_Duplicado</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Rejeitar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>End_Duplicado</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Exportar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gw_Fork</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Notificar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Registrar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gw_Join</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Confirmar</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_VerificarErros</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>End_ErroTratado</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_OCR" name="Módulo OCR / Webcam">
        <bpmn:flowNodeRef>Task_Escanear</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Extrair</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gw_Leitura</bpmn:flowNodeRef>
      </bpmn:lane>
    </bpmn:laneSet>

    <bpmn:startEvent id="Start_Malote" name="Malote recebido">
      <bpmn:messageEventDefinition id="MED_Malote" messageRef="Msg_Malote"/>
    </bpmn:startEvent>
    <bpmn:task id="Task_Abrir" name="Abrir malote e classificar documentos"/>
    <bpmn:task id="Task_Escanear" name="Escanear/fotografar boleto"/>
    <bpmn:task id="Task_Extrair" name="Extrair código de barras e valor via OCR"/>
    <bpmn:exclusiveGateway id="Gw_Leitura" name="Leitura OCR correta?"/>
    <bpmn:task id="Task_VerificarFalha" name="Verificar motivo da falha de leitura"/>
    <bpmn:task id="Task_Validar" name="Validar dados extraídos"/>
    <bpmn:exclusiveGateway id="Gw_Duplicado" name="Boleto duplicado?"/>
    <bpmn:task id="Task_Rejeitar" name="Rejeitar boleto duplicado"/>
    <bpmn:endEvent id="End_Duplicado" name="Boleto descartado"/>
    <bpmn:task id="Task_Exportar" name="Exportar lote para CSV"/>
    <bpmn:parallelGateway id="Gw_Fork" name="Abrir paralelo"/>
    <bpmn:task id="Task_Notificar" name="Notificar assistente para confirmar importação"/>
    <bpmn:task id="Task_Registrar" name="Registrar lote no livro fiscal"/>
    <bpmn:parallelGateway id="Gw_Join" name="Fechar paralelo"/>
    <bpmn:task id="Task_Confirmar" name="Confirmar importação no ERP"/>
    <bpmn:task id="Task_VerificarErros" name="Verificar possíveis erros de importação"/>
    <bpmn:endEvent id="End_ErroTratado" name="Erro verificado"/>
    <bpmn:textAnnotation id="Annot_Hotspot">
      <bpmn:text>Hotspot: o sistema deve permitir importar boletos duplicados no mesmo lote?</bpmn:text>
    </bpmn:textAnnotation>

    <bpmn:sequenceFlow id="F_E1" sourceRef="Start_Malote" targetRef="Task_Abrir"/>
    <bpmn:sequenceFlow id="F_E2" sourceRef="Task_Abrir" targetRef="Task_Escanear"/>
    <bpmn:sequenceFlow id="F_E3" sourceRef="Task_Escanear" targetRef="Task_Extrair"/>
    <bpmn:sequenceFlow id="F_E4" sourceRef="Task_Extrair" targetRef="Gw_Leitura"/>
    <bpmn:sequenceFlow id="F_E5" name="Não" sourceRef="Gw_Leitura" targetRef="Task_VerificarFalha"/>
    <bpmn:sequenceFlow id="F_E6" sourceRef="Task_VerificarFalha" targetRef="Task_Escanear"/>
    <bpmn:sequenceFlow id="F_E7" name="Sim" sourceRef="Gw_Leitura" targetRef="Task_Validar"/>
    <bpmn:sequenceFlow id="F_E8" sourceRef="Task_Validar" targetRef="Gw_Duplicado"/>
    <bpmn:sequenceFlow id="F_E9" name="Sim" sourceRef="Gw_Duplicado" targetRef="Task_Rejeitar"/>
    <bpmn:sequenceFlow id="F_E10" sourceRef="Task_Rejeitar" targetRef="End_Duplicado"/>
    <bpmn:sequenceFlow id="F_E11" name="Não" sourceRef="Gw_Duplicado" targetRef="Task_Exportar"/>
    <bpmn:sequenceFlow id="F_E12" sourceRef="Task_Exportar" targetRef="Gw_Fork"/>
    <bpmn:sequenceFlow id="F_E13" sourceRef="Gw_Fork" targetRef="Task_Notificar"/>
    <bpmn:sequenceFlow id="F_E14" sourceRef="Gw_Fork" targetRef="Task_Registrar"/>
    <bpmn:sequenceFlow id="F_E15" sourceRef="Task_Notificar" targetRef="Gw_Join"/>
    <bpmn:sequenceFlow id="F_E16" sourceRef="Task_Registrar" targetRef="Gw_Join"/>
    <bpmn:sequenceFlow id="F_E17" sourceRef="Gw_Join" targetRef="Task_Confirmar"/>
    <bpmn:sequenceFlow id="F_E18" sourceRef="Task_VerificarErros" targetRef="End_ErroTratado"/>
    <bpmn:association id="Assoc_1" sourceRef="Gw_Duplicado" targetRef="Annot_Hotspot"/>
  </bpmn:process>

  <!-- PISCINA: ERP -->
  <bpmn:process id="Proc_ERP" isExecutable="false">
    <bpmn:startEvent id="Start_ERP" name="CSV recebido">
      <bpmn:messageEventDefinition id="MED_CSV" messageRef="Msg_CSV"/>
    </bpmn:startEvent>
    <bpmn:task id="Task_Importar" name="Importar CSV no ERP Contábil"/>
    <bpmn:exclusiveGateway id="Gw_Importacao" name="Importação concluída?"/>
    <bpmn:endEvent id="End_OK" name="Lançamento contábil concluído"/>
    <bpmn:endEvent id="End_Falha" name="Importação não realizada">
      <bpmn:messageEventDefinition id="MED_Erro" messageRef="Msg_ErroImportacao"/>
    </bpmn:endEvent>

    <bpmn:sequenceFlow id="F_P1" sourceRef="Start_ERP" targetRef="Task_Importar"/>
    <bpmn:sequenceFlow id="F_P2" sourceRef="Task_Importar" targetRef="Gw_Importacao"/>
    <bpmn:sequenceFlow id="F_P3" name="Sim" sourceRef="Gw_Importacao" targetRef="End_OK"/>
    <bpmn:sequenceFlow id="F_P4" name="Não" sourceRef="Gw_Importacao" targetRef="End_Falha"/>
  </bpmn:process>

  <!-- DIAGRAMA (layout visual) -->
  <bpmndi:BPMNDiagram id="BPMNDiagram_Boletos">
    <bpmndi:BPMNPlane id="BPMNPlane_Boletos" bpmnElement="Collab_Boletos">

      <bpmndi:BPMNShape id="Part_Cliente_di" bpmnElement="Part_Cliente" isHorizontal="true">
        <dc:Bounds x="40" y="40" width="2200" height="120"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Part_Escritorio_di" bpmnElement="Part_Escritorio" isHorizontal="true">
        <dc:Bounds x="40" y="200" width="2200" height="480"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Part_ERP_di" bpmnElement="Part_ERP" isHorizontal="true">
        <dc:Bounds x="40" y="720" width="2200" height="180"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_Assistente_di" bpmnElement="Lane_Assistente" isHorizontal="true">
        <dc:Bounds x="70" y="200" width="2170" height="260"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_OCR_di" bpmnElement="Lane_OCR" isHorizontal="true">
        <dc:Bounds x="70" y="460" width="2170" height="220"/>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNShape id="Start_Cliente_di" bpmnElement="Start_Cliente">
        <dc:Bounds x="130" y="82" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Enviar_di" bpmnElement="Task_Enviar">
        <dc:Bounds x="220" y="60" width="120" height="80"/>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNShape id="Start_Malote_di" bpmnElement="Start_Malote">
        <dc:Bounds x="130" y="312" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Abrir_di" bpmnElement="Task_Abrir">
        <dc:Bounds x="210" y="290" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Escanear_di" bpmnElement="Task_Escanear">
        <dc:Bounds x="380" y="530" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Extrair_di" bpmnElement="Task_Extrair">
        <dc:Bounds x="550" y="530" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gw_Leitura_di" bpmnElement="Gw_Leitura" isMarkerVisible="true">
        <dc:Bounds x="710" y="545" width="50" height="50"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_VerificarFalha_di" bpmnElement="Task_VerificarFalha">
        <dc:Bounds x="540" y="290" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Validar_di" bpmnElement="Task_Validar">
        <dc:Bounds x="850" y="290" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gw_Duplicado_di" bpmnElement="Gw_Duplicado" isMarkerVisible="true">
        <dc:Bounds x="1010" y="305" width="50" height="50"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Rejeitar_di" bpmnElement="Task_Rejeitar">
        <dc:Bounds x="1100" y="210" width="120" height="60"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="End_Duplicado_di" bpmnElement="End_Duplicado">
        <dc:Bounds x="1280" y="222" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Exportar_di" bpmnElement="Task_Exportar">
        <dc:Bounds x="1110" y="290" width="120" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gw_Fork_di" bpmnElement="Gw_Fork">
        <dc:Bounds x="1270" y="305" width="50" height="50"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Notificar_di" bpmnElement="Task_Notificar">
        <dc:Bounds x="1340" y="210" width="150" height="70"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Registrar_di" bpmnElement="Task_Registrar">
        <dc:Bounds x="1340" y="370" width="150" height="70"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gw_Join_di" bpmnElement="Gw_Join">
        <dc:Bounds x="1530" y="305" width="50" height="50"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Confirmar_di" bpmnElement="Task_Confirmar">
        <dc:Bounds x="1600" y="290" width="130" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_VerificarErros_di" bpmnElement="Task_VerificarErros">
        <dc:Bounds x="1960" y="290" width="150" height="70"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="End_ErroTratado_di" bpmnElement="End_ErroTratado">
        <dc:Bounds x="1800" y="307" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Annot_Hotspot_di" bpmnElement="Annot_Hotspot">
        <dc:Bounds x="960" y="400" width="200" height="50"/>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNShape id="Start_ERP_di" bpmnElement="Start_ERP">
        <dc:Bounds x="1650" y="767" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Importar_di" bpmnElement="Task_Importar">
        <dc:Bounds x="1730" y="745" width="150" height="80"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gw_Importacao_di" bpmnElement="Gw_Importacao" isMarkerVisible="true">
        <dc:Bounds x="1930" y="760" width="50" height="50"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="End_OK_di" bpmnElement="End_OK">
        <dc:Bounds x="2040" y="767" width="36" height="36"/>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="End_Falha_di" bpmnElement="End_Falha">
        <dc:Bounds x="1937" y="840" width="36" height="36"/>
      </bpmndi:BPMNShape>

      <!-- Fluxos de sequência -->
      <bpmndi:BPMNEdge id="F_C1_di" bpmnElement="F_C1">
        <di:waypoint x="166" y="100"/><di:waypoint x="220" y="100"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E1_di" bpmnElement="F_E1">
        <di:waypoint x="166" y="330"/><di:waypoint x="210" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E2_di" bpmnElement="F_E2">
        <di:waypoint x="270" y="370"/><di:waypoint x="270" y="570"/><di:waypoint x="380" y="570"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E3_di" bpmnElement="F_E3">
        <di:waypoint x="500" y="570"/><di:waypoint x="550" y="570"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E4_di" bpmnElement="F_E4">
        <di:waypoint x="670" y="570"/><di:waypoint x="710" y="570"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E5_di" bpmnElement="F_E5">
        <di:waypoint x="735" y="545"/><di:waypoint x="735" y="330"/><di:waypoint x="660" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E6_di" bpmnElement="F_E6">
        <di:waypoint x="600" y="370"/><di:waypoint x="600" y="500"/><di:waypoint x="440" y="500"/><di:waypoint x="440" y="530"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E7_di" bpmnElement="F_E7">
        <di:waypoint x="760" y="570"/><di:waypoint x="910" y="570"/><di:waypoint x="910" y="370"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E8_di" bpmnElement="F_E8">
        <di:waypoint x="970" y="330"/><di:waypoint x="1010" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E9_di" bpmnElement="F_E9">
        <di:waypoint x="1035" y="305"/><di:waypoint x="1035" y="240"/><di:waypoint x="1100" y="240"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E10_di" bpmnElement="F_E10">
        <di:waypoint x="1220" y="240"/><di:waypoint x="1280" y="240"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E11_di" bpmnElement="F_E11">
        <di:waypoint x="1060" y="330"/><di:waypoint x="1110" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E12_di" bpmnElement="F_E12">
        <di:waypoint x="1230" y="330"/><di:waypoint x="1270" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E13_di" bpmnElement="F_E13">
        <di:waypoint x="1295" y="305"/><di:waypoint x="1295" y="245"/><di:waypoint x="1340" y="245"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E14_di" bpmnElement="F_E14">
        <di:waypoint x="1295" y="355"/><di:waypoint x="1295" y="405"/><di:waypoint x="1340" y="405"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E15_di" bpmnElement="F_E15">
        <di:waypoint x="1490" y="245"/><di:waypoint x="1555" y="245"/><di:waypoint x="1555" y="305"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E16_di" bpmnElement="F_E16">
        <di:waypoint x="1490" y="405"/><di:waypoint x="1555" y="405"/><di:waypoint x="1555" y="355"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E17_di" bpmnElement="F_E17">
        <di:waypoint x="1580" y="330"/><di:waypoint x="1600" y="330"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_E18_di" bpmnElement="F_E18">
        <di:waypoint x="1960" y="325"/><di:waypoint x="1836" y="325"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Assoc_1_di" bpmnElement="Assoc_1">
        <di:waypoint x="1035" y="355"/><di:waypoint x="1035" y="400"/>
      </bpmndi:BPMNEdge>

      <bpmndi:BPMNEdge id="F_P1_di" bpmnElement="F_P1">
        <di:waypoint x="1686" y="785"/><di:waypoint x="1730" y="785"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_P2_di" bpmnElement="F_P2">
        <di:waypoint x="1880" y="785"/><di:waypoint x="1930" y="785"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_P3_di" bpmnElement="F_P3">
        <di:waypoint x="1980" y="785"/><di:waypoint x="2040" y="785"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="F_P4_di" bpmnElement="F_P4">
        <di:waypoint x="1955" y="810"/><di:waypoint x="1955" y="840"/>
      </bpmndi:BPMNEdge>

      <!-- Fluxos de mensagem entre piscinas -->
      <bpmndi:BPMNEdge id="MF_Malote_di" bpmnElement="MF_Malote">
        <di:waypoint x="280" y="140"/><di:waypoint x="280" y="250"/><di:waypoint x="148" y="250"/><di:waypoint x="148" y="312"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="MF_CSV_di" bpmnElement="MF_CSV">
        <di:waypoint x="1665" y="370"/><di:waypoint x="1665" y="767"/>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="MF_Erro_di" bpmnElement="MF_Erro">
        <di:waypoint x="1973" y="858"/><di:waypoint x="2090" y="858"/><di:waypoint x="2090" y="360"/>
      </bpmndi:BPMNEdge>

    </bpmndi:BPMNPlane>
  </bpmndi:BPMNDiagram>
</bpmn:definitions>
