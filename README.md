private EmployeeStageEntity mapToEmployeeStageEntity(EmployeeViewEntity source) {
        EmployeeStageEntity destination = new EmployeeStageEntity();

        destination.setEmpIdNum(source.getSourceWorkerId());
        destination.setFullName(source.getFullName());
        destination.setFirstName(source.getFirstName());
        destination.setLastName(source.getLastName());
        destination.setMiddleName(source.getMiddleName());

        // Example hardcoded value for department, adjust as necessary
        destination.setEmployeeDept("DefaultDept"); 
        destination.setEmpIdn(source.getSourceNetworkId());

        return destination;
    }


    @Transactional
    public void bulkTransfer(List<EmployeeViewEntity> sourceEmployees) {
        int count = 0;

        for (EmployeeViewEntity source : sourceEmployees) {
            EmployeeStageEntity destination = mapToEmployeeStageEntity(source);

            entityManager.persist(destination);
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
    }
