using System.Data.SqlClient;

class Program {
    static void Main() {
        // Definir a string de conexão
        string connectionString = "Server=localhost;Database=banco;User Id=usuario;Password=senha;";

        // Criar uma conexão
        using (SqlConnection connection = new SqlConnection(connectionString)) {
            connection.Open();

            // Executar uma query
            string sql = "SELECT * FROM tabela";
            SqlCommand command = new SqlCommand(sql, connection);
            SqlDataReader reader = command.ExecuteReader();

            // Ler os resultados
            while (reader.Read()) {
                Console.WriteLine(reader["coluna"]);
            }

            reader.Close();
        }
    }
}
using Microsoft.EntityFrameworkCore;

public class BancoContext : DbContext {
    public DbSet<Tabela> Tabelas { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) {
        optionsBuilder.UseSqlServer("Server=localhost;Database=banco;User Id=usuario;Password=senha;");
    }
}

public class Tabela {
    public int Id { get; set; }
    public string Nome { get; set; }
}

class Program {
    static void Main() {
        using (var context = new BancoContext()) {
            // Criar uma nova tabela
            var tabela = new Tabela { Nome = "Nova tabela" };
            context.Tabelas.Add(tabela);
            context.SaveChanges();

            // Buscar todas as tabelas
            var tabelas = context.Tabelas.ToList();
            foreach (var t in tabelas) {
                Console.WriteLine(t.Nome);
            }
        }
    }
}

