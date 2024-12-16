@Repository
public class EmployeeStgRepository {

    @PersistenceContext
    private EntityManager entityManager;

    private static final int BATCH_SIZE = 50; // Adjust batch size as needed

    @Transactional
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
    }
}
