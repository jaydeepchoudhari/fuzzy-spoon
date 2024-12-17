 @Transactional
    public void bulkInsert(List<EmployeeStageEntity> employees) {
        int count = 0;
        String sql = "INSERT INTO EMPLOYEEstg (emp_id_num, full_user_name, first_name, last_name, middle_initial, employee_dept, emp_iden) " +
                     "VALUES (:empIdNum, :fullName, :firstName, :lastName, :middleName, :employeeDept, :empIdn)";

        for (EmployeeStageEntity employee : employees) {
            entityManager.createNativeQuery(sql)
                    .setParameter("empIdNum", employee.getEmpIdNum())
                    .setParameter("fullName", employee.getFullName())
                    .setParameter("firstName", employee.getFirstName())
                    .setParameter("lastName", employee.getLastName())
                    .setParameter("middleName", employee.getMiddleName())
                    .setParameter("employeeDept", employee.getEmployeeDept())
                    .setParameter("empIdn", employee.getEmpIdn())
                    .executeUpdate();
            count++;

            if (count % BATCH_SIZE == 0) {
                entityManager.flush();
                entityManager.clear();
            }
        }

        // Final flush and clear
        entityManager.flush();
        entityManager.clear();
    }
