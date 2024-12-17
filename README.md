@Autowired
    private JdbcTemplate jdbcTemplate;

    private static final int BATCH_SIZE = 5000; // Adjust batch size as needed

    @Transactional
    public void bulkInsert(List<EmployeeStageEntity> employees) {
        String baseSql = "INSERT INTO EMPLOYEEstg (emp_id_num, full_user_name, first_name, last_name, middle_initial, employee_dept, emp_iden) VALUES ";

        for (int i = 0; i < employees.size(); i += BATCH_SIZE) {
            int end = Math.min(i + BATCH_SIZE, employees.size());
            List<EmployeeStageEntity> batch = employees.subList(i, end);

            // Create bulk insert values
            String values = batch.stream()
                .map(employee -> String.format("('%s', '%s', '%s', '%s', '%s', '%s', '%s')",
                        escapeSql(employee.getEmpIdNum()),
                        escapeSql(employee.getFullName()),
                        escapeSql(employee.getFirstName()),
                        escapeSql(employee.getLastName()),
                        escapeSql(employee.getMiddleName()),
                        escapeSql(employee.getEmployeeDept()),
                        escapeSql(employee.getEmpIdn())))
                .collect(Collectors.joining(", "));

            String sql = baseSql + values;
            jdbcTemplate.execute(sql);
        }
    }

    // Utility method to escape single quotes in SQL values
    private String escapeSql(String value) {
        return value != null ? value.replace("'", "''") : "";
    }
