public void transfer(List<EmployeeViewEntity> sourceEmployees) {
        // Map source employees to stage entities
        List<EmployeeStageEntity> stageEntities = sourceEmployees.stream()
                .map(this::mapToEmployeeStageEntity)
                .toList();

        // Call repository for bulk insert
        employeeStgRepository.bulkInsert(stageEntities);
    }

    private EmployeeStageEntity mapToEmployeeStageEntity(EmployeeViewEntity source) {
        EmployeeStageEntity destination = new EmployeeStageEntity();

        destination.setEmpIdNum(source.getSourceWorkerId());
        destination.setFullName(source.getFullName());
        destination.setFirstName(source.getFirstName());
        destination.setLastName(source.getLastName());
        destination.setMiddleName(source.getMiddleName());
        destination.setEmployeeDept("DefaultDept");
        destination.setEmpIdn(source.getSourceNetworkId());

        return destination;
    }
