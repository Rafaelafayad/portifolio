# portfolio.py

import json
import os

class Portfolio:
    def __init__(self):
        self.filename = 'portfolio.json'
        if not os.path.exists(self.filename):
            self.save_data([])

    def save_data(self, data):
        with open(self.filename, 'w') as file:
            json.dump(data, file, indent=4)

    def load_data(self):
        with open(self.filename, 'r') as file:
            return json.load(file)

    def add_project(self, title, description):
        projects = self.load_data()
        project = {
            'title': title,
            'description': description
        }
        projects.append(project)
        self.save_data(projects)
        print(f'Projeto "{title}" adicionado com sucesso!')

    def list_projects(self):
        projects = self.load_data()
        if not projects:
            print("Nenhum projeto encontrado.")
            return
        print("Projetos no portfólio:")
        for idx, project in enumerate(projects, start=1):
            print(f"{idx}. {project['title']}: {project['description']}")

    def remove_project(self, index):
        projects = self.load_data()
        if index < 1 or index > len(projects):
            print("Projeto não encontrado.")
            return
        removed = projects.pop(index - 1)
        self.save_data(projects)
        print(f"Projeto '{removed['title']}' removido com sucesso!")

    def edit_project(self, index, new_title, new_description):
        projects = self.load_data()
        if index < 1 or index > len(projects):
            print("Projeto não encontrado.")
            return
        projects[index - 1] = {'title': new_title, 'description': new_description}
        self.save_data(projects)
        print(f"Projeto atualizado para '{new_title}' com sucesso!")

def main():
    portfolio = Portfolio()

    while True:
        print("\nBem-vindo ao seu Portfólio!")
        print("1. Adicionar projeto")
        print("2. Listar projetos")
        print("3. Remover projeto")
        print("4. Editar projeto")
        print("5. Sair")

        choice = input("Escolha uma opção: ")

        if choice == '1':
            title = input("Digite o título do projeto: ")
            description = input("Digite a descrição do projeto: ")
            portfolio.add_project(title, description)
        elif choice == '2':
            portfolio.list_projects()
        elif choice == '3':
            index = int(input("Digite o número do projeto a ser removido: "))
            portfolio.remove_project(index)
        elif choice == '4':
            index = int(input("Digite o número do projeto a ser editado: "))
            new_title = input("Digite o novo título do projeto: ")
            new_description = input("Digite a nova descrição do projeto: ")
            portfolio.edit_project(index, new_title, new_description)
        elif choice == '5':
            print("Saindo...")
            break
        else:
            print("Opção inválida!")

if __name__ == '__main__':
    main()
