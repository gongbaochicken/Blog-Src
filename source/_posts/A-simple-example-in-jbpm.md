title: A simple example in jbpm
date: 2015-06-09 10:43:24
tags:
  - Java
categories:
  - Workflow
---
In some sense, JBPM is simple, because its goal is to offer process management features in a way that both business users and developers like it. Here I will give you a very simple JBPM example with Java action codeblock.
<!--more-->

### JPDL
There is a JBoss jBPM Process Definition Language file, which defines the topology of the application process.

![test1.jpdl.xml](http://7xjjbh.com1.z0.glb.clouddn.com/blog1.jpg)

Suppose one guy("Tom") wants to apply for something, he has to submit his application online at the "Apply" task. Then the application form will moves to a certain assignee(Here I name it is "manager"). If the manager says "Yes", the process moves to the end, and the application successfully ends. If the manager says "No", the process moves back the "Apply" step, and Tom should apply again (Poor Tom~~).

Well, that's the whole process. Simple?

Next, I will show the xml code of the JPDL file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<process key="test" name="test1" xmlns="http://jbpm.org/4.4/jpdl">   <start g="126,-12,48,48" name="start1">      <transition g="-47,-17" name="submit" to="Apply"/>   </start>   <task assignee="Tom" form="Servlet?opt=toApply" g="101,67,92,52" name="Apply">      <transition g="-67,-1" name="to check" to="check"/>   </task>   <task assignee="manager" form="Servlet?opt=toManagerApprove" g="100,155,92,52" name="check">      <transition g="332,180;331,95:5,-10" name="No" to="Apply"/>      <transition g="-43,-14" name="Yes" to="end1"/>   </task>   <end g="121,250,48,48" name="end1"/></process>
```


### Java Code
Here is the java code that driving the actions of the whole process above. The given code is pretty straightforward. However, the JPDL is more complicated in industry. So you may have more concerns.

```java
package com.tgb.video;import java.util.HashMap;import java.util.Iterator;import java.util.List;import java.util.Map;import java.util.Set;import org.jbpm.api.Configuration;import org.jbpm.api.ExecutionService;import org.jbpm.api.HistoryService;import org.jbpm.api.IdentityService;import org.jbpm.api.ManagementService;import org.jbpm.api.ProcessEngine;import org.jbpm.api.ProcessInstance;import org.jbpm.api.RepositoryService;import org.jbpm.api.TaskService;import org.jbpm.api.task.Task;

public class JbpmProcess {
	protected RepositoryService repositoryService;
	protected ExecutionService executionService;
	protected TaskService taskService;
	protected String processInstanceID;
	public void deploy() {		ProcessEngine processEngine = Configuration.getProcessEngine();		repositoryService = processEngine.getRepositoryService();
		repositoryService.createDeployment().addResourceFromClasspath("test1.jpdl.xml").deploy();	}
	public void createInstance() {		ProcessEngine processEngine = Configuration.getProcessEngine();		executionService = processEngine.getExecutionService();		ProcessInstance processInstance = executionService.startProcessInstanceByKey("test");		processInstanceID = processInstance.getId();		System.out.println("ID:" + processInstanceID);	}
	public String getCurrentInstanceID(){		return processInstanceID;	}
	public ProcessInstance getProcessInstanceByTaskId(String taskId) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		String executionId = taskService.getTask(taskId).getExecutionId();		return (ProcessInstance) executionService.findExecutionById(executionId).getProcessInstance();	}
	public List<ProcessInstance> getProcesInstanceList() {		ProcessEngine processEngine = Configuration.getProcessEngine();		List<ProcessInstance> procesInstanceList = processEngine.getExecutionService().createProcessInstanceQuery().list();		return procesInstanceList;	}
	public String getProcessDefinitionForm(String taskId) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		Task task = taskService.getTask(taskId);		return task.getFormResourceName();	}
	public String  getCurrentActivityName(String processID) {		ProcessEngine processEngine = Configuration.getProcessEngine();		executionService = processEngine.getExecutionService();		String activityName = executionService.createProcessInstanceQuery().processInstanceId(processID).uniqueResult().findActiveActivityNames().toString();		//System.out.println("current activity: " + activityName);		return activityName;	}
	public void getTask(String assignee) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		List<Task> tasks = taskService.findPersonalTasks(assignee);		if(tasks.size() != 0){			for(int i = 0; i < tasks.size(); i++){				Task task = tasks.get(i);				System.out.println("Task Num:" + tasks.size());				System.out.println("Task:" + task.getActivityName());				System.out.println("People:" + task.getAssignee() + ", Task ID: " + task.getId());			}		}	}
	public void addTaskVariable(String TaskID, String TaskUserID, String TaskUserName){		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		Map<String, Object> map = new HashMap<String, Object>();		map.put("TaskUserID", TaskUserID);		map.put("TaskUserName", TaskUserName);		taskService.setVariables(TaskID, map);	}		
	public String getTaskVariableID(String TaskID) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		String taskUserID = taskService.getVariable(TaskID, "TaskUserID").toString();		//System.out.println("Get Task User ID:  " +  taskUserID)		return taskUserID;	}
	public String getTaskVariableName(String TaskID) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		String taskUserName = taskService.getVariable(TaskID, "TaskUserName").toString();		//System.out.println("Get Task User name:  "+ taskUserName);		return taskUserName;	}
	public void completeTask(String TaskID) {		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		taskService.completeTask(TaskID);	}
	public void getProcessInstanceVariable(String ProcessID){		ProcessEngine processEngine = Configuration.getProcessEngine();		executionService = processEngine.getExecutionService();		String userID = executionService.getVariable(ProcessID, "UserID").toString();		String userName = executionService.getVariable(ProcessID, "UserName").toString();		System.out.println("Get User ID: " + userID);		System.out.println("Get User name: "+ userName);	}
	public void updateVariable(String ProcessID, String UserID, String UserName){		ProcessEngine processEngine = Configuration.getProcessEngine();		executionService = processEngine.getExecutionService();		executionService.setVariable(ProcessID, "UserID", UserID);		executionService.setVariable(ProcessID, "UserName", UserName);	}
	//If Yes, the processInstance moves to the end. If the manager replies "No",  the processInstance goes back to application step  	public void moveAfterCheck(String taskID, String result){		ProcessEngine processEngine = Configuration.getProcessEngine();		taskService = processEngine.getTaskService();		if(result.equals("Yes")){			//Go to the end			taskService.completeTask(taskID, "Yes");		}		else if(result.equals("No")){			//Feedback			taskService.completeTask(taskID, "No");		}
		else{			System.out.print("The result has error.");		}	}	/*	 * History record is not needed at this time	 *
	 * */
}
```

### Test
After finishing these steps, you can use JUnit to test your action class. But before that, you need to re-configure the jbpm.hibernate.cfg.xml(import into eclipse).

```
<?xml version="1.0" encoding="utf-8"?><!DOCTYPE hibernate-configuration PUBLIC          "-//Hibernate/Hibernate Configuration DTD 3.0//EN"          "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd"><hibernate-configuration>  <session-factory>
     <property name="hibernate.dialect">org.hibernate.dialect.MySQLInnoDBDialect</property>     <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>     <property name="hibernate.connection.url">jdbc:mysql://127.0.0.1:3306/jbpmtest?useUnicode=true&amp;characterEncoding=UTF-8</property>     <property name="hibernate.connection.username">XXX</property>     <property name="hibernate.connection.password">XXX</property>     <property name="hibernate.hbm2ddl.auto">update</property>     <property name="hibernate.format_sql">true</property>
     <mapping resource="jbpm.repository.hbm.xml" />     <mapping resource="jbpm.execution.hbm.xml" />     <mapping resource="jbpm.history.hbm.xml" />     <mapping resource="jbpm.task.hbm.xml" />     <mapping resource="jbpm.identity.hbm.xml" />
  </session-factory></hibernate-configuration>

```

Here, I choose MySQL as the jbpm database, and name & username & password you can set as you wish.

### Tips:

1. The first time you deploy the jbpm in your db, there is no table. Therefore, in jbpm.hibernate.cfg.xml, use "create" rather than "update" in the first time. Then you can change back to "update".

2. If the error is about the sql dialect, change "org.hibernate.dialect.MySQLInnoDBDialect" into "org.hibernate.dialect.MySQLDialect".

3. Don't forge to keep your database server on.
