# Personalizando-Acessos-e-Automatizando-a-es-no-MySQL
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.sql.Statement;

public class MySQLAccessAutomation {

    // URL de conexão com o banco de dados
    private static final String DB_URL = "jdbc:mysql://localhost:3306/seu_banco_de_dados";
    private static final String USER = "seu_usuario";
    private static final String PASS = "sua_senha";

    public static void main(String[] args) {
        try {
            // Conectar ao banco de dados
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASS);
            System.out.println("Conexão estabelecida com sucesso!");

            // Criar um novo usuário com permissões específicas
            createUser (connection, "novo_usuario", "senha_usuario");

            // Automatizar uma ação: inserir dados em uma tabela
            insertData(connection, "sua_tabela", "valor1", "valor2");

            // Fechar a conexão
            connection.close();
            System.out.println("Conexão fechada.");

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void createUser (Connection connection, String username, String password) throws SQLException {
        String sql = "CREATE USER ?@'localhost' IDENTIFIED BY ?";
        try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
            preparedStatement.setString(1, username);
            preparedStatement.setString(2, password);
            preparedStatement.executeUpdate();
            System.out.println("Usuário criado com sucesso!");
        }
    }

    private static void insertData(Connection connection, String tableName, String value1, String value2) throws SQLException {
        String sql = "INSERT INTO " + tableName + " (coluna1, coluna2) VALUES (?, ?)";
        try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
            preparedStatement.setString(1, value1);
            preparedStatement.setString(2, value2);
            preparedStatement.executeUpdate();
            System.out.println("Dados inseridos com sucesso!");
        }
    }
}

Explicação do Código
Conexão com o Banco de Dados: O código usa DriverManager para estabelecer uma conexão com o banco de dados MySQL.
Criar Usuário: A função createUser  cria um novo usuário no banco de dados com a senha especificada. O uso de PreparedStatement ajuda a prevenir SQL Injection.
Inserir Dados: A função insertData insere valores em uma tabela específica. Você deve substituir sua_tabela, coluna1, e coluna2 pelos nomes apropriados do seu banco de dados.
