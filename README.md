# petshop-gerenciamento

// Classe abstrata Animal
abstract class Animal {
    protected String nome;
    protected String racaTipo;
    protected int idade;
    protected String proprietario;

    public Animal(String nome, String racaTipo, int idade, String proprietario) {
        this.nome = nome;
        this.racaTipo = racaTipo;
        this.idade = idade;
        this.proprietario = proprietario;
    }

    @Override
    public String toString() {
        return nome + ", " + racaTipo + ", idade: " + idade + ", proprietário: " + proprietario;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Animal animal = (Animal) obj;
        return idade == animal.idade && nome.equals(animal.nome) && racaTipo.equals(animal.racaTipo) && proprietario.equals(animal.proprietario);
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(nome, racaTipo, idade, proprietario);
    }
}

// Subclasse Cachorro
class Cachorro extends Animal {
    private String porte;

    public Cachorro(String nome, String racaTipo, int idade, String proprietario, String porte) {
        super(nome, racaTipo, idade, proprietario);
        this.porte = porte;
    }

    public String getPorte() {
        return porte;
    }

    @Override
    public String toString() {
        return "Cachorro: " + super.toString() + ", porte: " + porte;
    }

    @Override
    public boolean equals(Object obj) {
        return super.equals(obj) && ((Cachorro) obj).porte.equals(this.porte);
    }

    @Override
    public int hashCode() {
        return super.hashCode() + porte.hashCode();
    }
}

// Subclasse Gato
class Gato extends Animal {
    private String corOlhos;

    public Gato(String nome, String racaTipo, int idade, String proprietario, String corOlhos) {
        super(nome, racaTipo, idade, proprietario);
        this.corOlhos = corOlhos;
    }

    @Override
    public String toString() {
        return "Gato: " + super.toString() + ", cor dos olhos: " + corOlhos;
    }

    @Override
    public boolean equals(Object obj) {
        return super.equals(obj) && ((Gato) obj).corOlhos.equals(this.corOlhos);
    }

    @Override
    public int hashCode() {
        return super.hashCode() + corOlhos.hashCode();
    }
}

// Subclasse OutroAnimal
class OutroAnimal extends Animal {
    private String tipoEspecifico;

    public OutroAnimal(String nome, String racaTipo, int idade, String proprietario, String tipoEspecifico) {
        super(nome, racaTipo, idade, proprietario);
        this.tipoEspecifico = tipoEspecifico;
    }

    @Override
    public String toString() {
        return "Outro Animal: " + super.toString() + ", tipo específico: " + tipoEspecifico;
    }

    @Override
    public boolean equals(Object obj) {
        return super.equals(obj) && ((OutroAnimal) obj).tipoEspecifico.equals(this.tipoEspecifico);
    }

    @Override
    public int hashCode() {
        return super.hashCode() + tipoEspecifico.hashCode();
    }
}

// Classe abstrata Servico
abstract class Servico {
    protected String nome;
    protected double precoBase;

    public Servico(String nome, double precoBase) {
        this.nome = nome;
        this.precoBase = precoBase;
    }

    public abstract double calcularPreco(Animal animal);

    @Override
    public String toString() {
        return nome + " - Preço base: R$" + precoBase;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Servico servico = (Servico) obj;
        return nome.equals(servico.nome) && precoBase == servico.precoBase;
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(nome, precoBase);
    }
}

// Subclasse Banho
class Banho extends Servico {
    public Banho() {
        super("Banho", 30.0);
    }

    @Override
    public double calcularPreco(Animal animal) {
        if (animal instanceof Cachorro) {
            String porte = ((Cachorro) animal).getPorte();
            switch (porte.toLowerCase()) {
                case "pequeno": return precoBase;
                case "medio": return precoBase + 10;
                case "grande": return precoBase + 20;
            }
        }
        return precoBase;
    }
}

// Subclasse Tosa
class Tosa extends Servico {
    public Tosa() {
        super("Tosa", 40.0);
    }

    @Override
    public double calcularPreco(Animal animal) {
        if (animal.racaTipo.equalsIgnoreCase("persa")) {
            return precoBase + 15;
        }
        return precoBase;
    }
}

// Subclasse ConsultaVeterinaria
class ConsultaVeterinaria extends Servico {
    public ConsultaVeterinaria() {
        super("Consulta Veterinária", 80.0);
    }

    @Override
    public double calcularPreco(Animal animal) {
        return precoBase; // Preço fixo
    }
}

// Classe GerenciadorPetshop
public class GerenciadorPetshop {
    public static void main(String[] args) {
        Animal dog = new Cachorro("Rex", "Labrador", 5, "João", "grande");
        Animal cat = new Gato("Mimi", "Persa", 3, "Ana", "Verde");
        Animal coelho = new OutroAnimal("Bunny", "Mini Lop", 2, "Carlos", "Coelho");

        Servico banho = new Banho();
        Servico tosa = new Tosa();
        Servico consulta = new ConsultaVeterinaria();

        System.out.println(dog);
        System.out.println("Preço do banho: R$" + banho.calcularPreco(dog));
        System.out.println("Preço da tosa: R$" + tosa.calcularPreco(dog));
        System.out.println("Preço da consulta: R$" + consulta.calcularPreco(dog));

        System.out.println("\n" + cat);
        System.out.println("Preço do banho: R$" + banho.calcularPreco(cat));
        System.out.println("Preço da tosa: R$" + tosa.calcularPreco(cat));
        System.out.println("Preço da consulta: R$" + consulta.calcularPreco(cat));

        System.out.println("\nComparando animais:");
        System.out.println("dog.equals(cat)? " + dog.equals(cat));
        System.out.println("hashCode do dog: " + dog.hashCode());
        System.out.println("hashCode do cat: " + cat.hashCode());
    }
}
