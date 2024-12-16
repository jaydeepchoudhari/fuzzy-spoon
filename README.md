package com.dukeenergy.formula.model.entities;

import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "EMPLOYEEstg")
@Data
@NoArgsConstructor
public class EmployeeStageEntity {
    @Id
    @Column(name = "emp_id_num")
    String empIdNum;
    @Column(name = "full_user_name")
    String fullName;
    @Column(name = "first_name")
    String firstName;
    @Column(name = "last_name")
    String lastName;
    @Column(name = "middle_initial")
    String middleName;
    @Column(name = "employee_dept")
    String employeeDept;
    @Column(name = "emp_iden")
    String empIdn;
}




package com.dukeenergy.formula.model.entities;

import lombok.Data;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Data
@Entity
@Table(name = "WorkForce.Worker_Info")
public class EmployeeViewEntity {
    @Id
    @Column(name = "Source_Worker_Id")
    String sourceWorkerId;
    @Column(name = "Full_Name")
    String fullName;
    @Column(name = "First_Name")
    String firstName;
    @Column(name = "Last_Name")
    String lastName;
    @Column(name = "Middle_Name")
    String middleName;
    @Column(name = "Source_Network_Id")
    String sourceNetworkId;
}
