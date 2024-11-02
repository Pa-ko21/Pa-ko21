<?xml version="1.0" encoding="UTF-8"?>
<bpmn:definitions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                  xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL"
                  xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI"
                  xmlns:dc="http://www.omg.org/spec/DD/20100524/DC"
                  xmlns:di="http://www.omg.org/spec/DD/20100524/DI"
                  id="Smart_eDA_Process" 
                  targetNamespace="http://bpmn.io/schema/bpmn">
    
  <!-- Process Definition -->
  <bpmn:process id="SmartElectronicDevelopmentAssessment" name="Smart eDA Process" isExecutable="true">
  
    <!-- Swimlanes -->
    <bpmn:laneSet>
      <bpmn:lane id="lane_Applicant" name="Applicant"/>
      <bpmn:lane id="lane_AssessmentManager" name="Assessment Manager"/>
      <bpmn:lane id="lane_Cadastre" name="Cadastre"/>
      <bpmn:lane id="lane_CityCouncil" name="City Council"/>
      <bpmn:lane id="lane_DoR" name="Department of Roads (DoR)"/>
      <bpmn:lane id="lane_NRW" name="Department of Natural Resources and Water (NRW)"/>
      <bpmn:lane id="lane_EPA" name="Environmental Protection Agency (EPA)"/>
    </bpmn:laneSet>

    <!-- Start Event -->
    <bpmn:startEvent id="StartEvent_ApplicationSubmission" name="Application Submission">
      <bpmn:outgoing>Flow_1</bpmn:outgoing>
    </bpmn:startEvent>

    <!-- Applicant Activities -->
    <bpmn:task id="Task_ReviewQuote" name="Review and Accept/Reject Quote" lane="lane_Applicant">
      <bpmn:incoming>Flow_4</bpmn:incoming>
      <bpmn:outgoing>Flow_5</bpmn:outgoing>
    </bpmn:task>
    
    <!-- Assessment Manager Activities -->
    <bpmn:task id="Task_RetrieveGeographicInfo" name="Retrieve Geographic Information" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_1</bpmn:incoming>
      <bpmn:outgoing>Flow_2</bpmn:outgoing>
    </bpmn:task>
    
    <bpmn:task id="Task_CalculateProcessingCosts" name="Calculate Processing Costs" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_3</bpmn:incoming>
      <bpmn:outgoing>Flow_4</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_RequestConflictCheck_DoR" name="Request Conflict Check with DoR" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_5</bpmn:incoming>
      <bpmn:outgoing>Flow_6</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_RequestLandAlterationPermit" name="Request Land Alteration Permit" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_8</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_RequestEnvironmentalLicense" name="Request Environmental License" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_9</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_FinalApprovalNotification" name="Send Final Approval Notification" lane="lane_AssessmentManager">
      <bpmn:incoming>Flow_10</bpmn:incoming>
      <bpmn:outgoing>Flow_11</bpmn:outgoing>
    </bpmn:task>

    <!-- Cadastre Activity -->
    <bpmn:task id="Task_ProvideGeographicInfo" name="Provide Geographic Information" lane="lane_Cadastre">
      <bpmn:incoming>Flow_2</bpmn:incoming>
      <bpmn:outgoing>Flow_3</bpmn:outgoing>
    </bpmn:task>

    <!-- City Council Activity -->
    <bpmn:task id="Task_ValidateProposal" name="Validate Development Proposal" lane="lane_CityCouncil">
      <bpmn:incoming>Flow_3</bpmn:incoming>
      <bpmn:outgoing>Flow_12</bpmn:outgoing>
    </bpmn:task>
    
    <!-- DoR Activity -->
    <bpmn:task id="Task_CheckRoadConflicts" name="Check for Road Development Conflicts" lane="lane_DoR">
      <bpmn:incoming>Flow_6</bpmn:incoming>
      <bpmn:outgoing>Flow_7</bpmn:outgoing>
    </bpmn:task>

    <!-- NRW Activity -->
    <bpmn:task id="Task_IssueLandAlterationPermit" name="Issue Land Alteration Permit" lane="lane_NRW">
      <bpmn:incoming>Flow_8</bpmn:incoming>
      <bpmn:outgoing>Flow_10</bpmn:outgoing>
    </bpmn:task>

    <!-- EPA Activity -->
    <bpmn:task id="Task_IssueEnvironmentalLicense" name="Issue Environmental License" lane="lane_EPA">
      <bpmn:incoming>Flow_9</bpmn:incoming>
      <bpmn:outgoing>Flow_10</bpmn:outgoing>
    </bpmn:task>

    <!-- Gateways -->
    <bpmn:exclusiveGateway id="Gateway_QuoteAcceptance" name="Quote Acceptance">
      <bpmn:incoming>Flow_5</bpmn:incoming>
      <bpmn:outgoing>Flow_6</bpmn:outgoing>
    </bpmn:exclusiveGateway>

    <bpmn:exclusiveGateway id="Gateway_ConflictCheck" name="Conflict Check">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_8</bpmn:outgoing>
      <bpmn:outgoing>Flow_12</bpmn:outgoing>
    </bpmn:exclusiveGateway>

    <bpmn:parallelGateway id="Gateway_PermitsParallel" name="Parallel Permit Requests">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_8</bpmn:outgoing>
      <bpmn:outgoing>Flow_9</bpmn:outgoing>
    </bpmn:parallelGateway>

    <!-- End Event -->
    <bpmn:endEvent id="EndEvent_FinalApproval" name="Final Approval Notification">
      <bpmn:incoming>Flow_11</bpmn:incoming>
    </bpmn:endEvent>

    <!-- Sequence Flows -->
    <bpmn:sequenceFlow id="Flow_1" sourceRef="StartEvent_ApplicationSubmission" targetRef="Task_RetrieveGeographicInfo"/>
    <bpmn:sequenceFlow id="Flow_2" sourceRef="Task_RetrieveGeographicInfo" targetRef="Task_ProvideGeographicInfo"/>
    <bpmn:sequenceFlow id="Flow_3" sourceRef="Task_ProvideGeographicInfo" targetRef="Task_ValidateProposal"/>
    <bpmn:sequenceFlow id="Flow_4" sourceRef="Task_ValidateProposal" targetRef="Task_CalculateProcessingCosts"/>
    <bpmn:sequenceFlow id="Flow_5" sourceRef="Task_CalculateProcessingCosts" targetRef="Task_ReviewQuote"/>
    <bpmn:sequenceFlow id="Flow_6" sourceRef="Task_ReviewQuote" targetRef="Gateway_QuoteAcceptance"/>
    <bpmn:sequenceFlow id="Flow_7" sourceRef="Gateway_QuoteAcceptance" targetRef="Task_RequestConflictCheck_DoR"/>
    <bpmn:sequenceFlow id="Flow_8" sourceRef="Gateway_ConflictCheck" targetRef="Task_RequestLandAlterationPermit"/>
    <bpmn:sequenceFlow id="Flow_9" sourceRef="Gateway_PermitsParallel" targetRef="Task_RequestEnvironmentalLicense"/>
    <bpmn:sequenceFlow id="Flow_10" sourceRef="Task_IssueLandAlterationPermit" targetRef="Task_FinalApprovalNotification"/>
    <bpmn:sequenceFlow id="Flow_11" sourceRef="Task_FinalApprovalNotification" targetRef="EndEvent_FinalApproval"/>
    <bpmn:sequenceFlow id="Flow_12" sourceRef="Task_CheckRoadConflicts" targetRef="Gateway_ConflictCheck"/>

  </bpmn:process>
  
</bpmn:definitions>
