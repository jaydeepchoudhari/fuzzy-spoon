@Async
    @Scheduled(cron = cron)
    void updateEmployees() {
        logger.info("Running CRON job: Update employees");
        employeeService.updateEmployees();
        logger.info("Ended CRON job: Update employees");
    }

    @Override
    @Transactional
    public void updateEmployees() {
        logger.info("Beginning Employee sync...");


        employeeRepository.DeleteEmpStageData();

        List<EmployeeViewEntity> sourceEmployees = empRepository.getAllActiveEmployees();

        List<EmployeeStageEntity> stageEntities = sourceEmployees.stream()
                .map(this::mapToEmployeeStageEntity)
                .toList();

        /*int BATCH_SIZE = 1000;

        for (int i = 0; i < stageEntities.size(); i += BATCH_SIZE) {
            int end = Math.min(i + BATCH_SIZE, stageEntities.size());
            List<EmployeeStageEntity> batch = stageEntities.subList(i, end);
            employeeStageRepository.saveAll(batch);
            employeeStageRepository.flush(); // Flush after each batch
        }*/

        //employeeStageRepository.saveAll(stageEntities);
        employeeStageRepository.bulkInsert(stageEntities);




    }

    @Repository
public class EmployeeStageRepository { //extends JpaRepository<EmployeeStageEntity, String>  {

    @PersistenceContext
    private EntityManager entityManager;

    private static final int BATCH_SIZE = 5000; // Adjust batch size as needed

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

   /* @Transactional
    public void bulkInsert(List<EmployeeStageEntity> employees) {
        int count = 0;

        for (EmployeeStageEntity employee : employees) {
            entityManager.persist(employee);
            count++;

            // Flush and clear the persistence context every BATCH_SIZE inserts
            if (count % BATCH_SIZE == 0) {
                entityManager.flush();
                entityManager.clear();
            }
        }

        // Flush remaining entities
        entityManager.flush();
        entityManager.clear();
    }*/




}
