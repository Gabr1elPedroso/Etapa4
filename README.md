package login;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * Classe responsável pela conexão com o banco de dados e validação de usuários.
 */
public class User {

    /**
     * Estabelece a conexão com o banco de dados MySQL.
     * 
     * @return Objeto Connection, ou null em caso de falha.
     */
    public Connection conectarBD() {
        Connection conn = null;
        try {
            // Carregar o driver JDBC do MySQL
            Class.forName("com.mysql.Driver").newInstance();

            // URL de conexão ao banco de dados
            String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";

            // Estabelecer a conexão
            conn = DriverManager.getConnection(url);

        } catch (Exception e) {
            // Tratamento de erro durante a conexão
            System.out.println("Erro ao conectar ao banco de dados: " + e.getMessage());
        }

        // Retornar o objeto Connection
        return conn;
    }

    /**
     * Nome do usuário autenticado (caso encontrado no banco de dados).
     */
    public String nome = "";

    /**
     * Resultado da validação de credenciais.
     */
    public boolean result = false;

    /**
     * Valida as credenciais de login e senha fornecidas.
     * 
     * @param login Login do usuário.
     * @param senha Senha do usuário.
     * @return true se o login for bem-sucedido, false caso contrário.
     */
    public boolean verificarUsuario(String login, String senha) {
        String sql = "";
        Connection conn = conectarBD();

        // Montagem da instrução SQL
        sql = "SELECT nome FROM usuarios WHERE login = '" + login + "' AND senha = '" + senha + "';";

        try {
            // Criação do Statement
            Statement st = conn.createStatement();

            // Execução da consulta SQL
            ResultSet rs = st.executeQuery(sql);

            // Verifica se o usuário foi encontrado
            if (rs.next()) {
                result = true;
                nome = rs.getString("nome");
            }
        } catch (Exception e) {
            // Tratamento de erro durante a execução da consulta
            System.out.println("Erro ao executar a consulta: " + e.getMessage());
        }

        // Retorna o resultado da validação
        return result;
    }
}
