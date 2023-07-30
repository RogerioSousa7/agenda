# agenda
Agenda usando python e streamlit
from datetime import date
import streamlit as st


# Função para adicionar um novo contato à lista
def adicionar_contato():
    st.header('Adicionar contato')
    email = st.text_input('Digite o E-mail:')
    if len(contatos) > 0:
        for contato in contatos:
            if email == contato['email']:
                st.error('Este contato já existe.')
                return

    nome = st.text_input('Nome:')
    sobrenome = st.text_input('Sobrenome:')
    telefone = st.text_input('Telefone:')

    contatos.append({
        'email': email.lower(),
        'nome': nome.strip().capitalize(),
        'sobrenome': sobrenome.strip().capitalize(),
        'telefone': telefone.strip(),
        'data': date.today().strftime('%d/%m/%Y')
    })
    st.success('Contato adicionado com sucesso!')


# Função para alterar informações de um contato existente
def alterar_contato():
    if len(contatos) > 0:
        email = st.text_input('Digite o e-mail do contato que deseja alterar:')
        for contato in contatos:
            if contato['email'] == email:
                st.write(f"Nome do contato: {contato['nome']}")
                st.write(f"Telefone: {contato['telefone']}")
                escolha = st.selectbox('O que deseja alterar?', ('Nome', 'Telefone'))
                if escolha == 'Nome':
                    novo_nome = st.text_input('Digite um novo nome para o contato:')
                    contato['nome'] = novo_nome.strip().capitalize()
                    st.success('Nome alterado com sucesso!')
                elif escolha == 'Telefone':
                    novo_tel = st.text_input('Digite um novo telefone para o contato:')
                    contato['telefone'] = novo_tel.strip()
                    st.success('Telefone alterado com sucesso!')
                return

        st.error('Não existe usuário cadastrado com o e-mail informado.')
    else:
        st.warning('Não há contatos registrados na agenda.')


# Função para procurar um contato pelo e-mail
def procurar_contato():
    if len(contatos) > 0:
        email = st.text_input('Digite o e-mail do contato:')
        for contato in contatos:
            if contato['email'] == email:
                st.write(f"Nome: {contato['nome']} {contato['sobrenome']}")
                st.write(f"Telefone: {contato['telefone']}")
                st.write(f"Data de registro: {contato['data']}")
                return

        st.warning('Contato não encontrado.')
    else:
        st.warning('Não há contatos registrados na agenda.')


# Função para remover um contato pelo e-mail
def remover_contato():
    if len(contatos) > 0:
        email = st.text_input('Digite o e-mail do contato que deseja remover:')
        x = 0
        while x < len(contatos):
            if contatos[x]['email'] == email:
                contatos.remove(contatos[x])
                st.success('Contato removido com sucesso!')
                return
            x += 1

        st.warning('Contato não encontrado.')
    else:
        st.warning('Não há contatos registrados na agenda.')


# Função para visualizar todos os contatos ordenados por nome
def ver_contatos():
    if len(contatos) > 0:
        contatos_ordenados = sorted(contatos, key=lambda contato: contato['nome'] + ' ' + contato['sobrenome'])
        for indice, contato in enumerate(contatos_ordenados, start=1):
            st.subheader(f'Contato {indice}')
            st.write(f"Nome: {contato['nome']} {contato['sobrenome']}")
            st.write(f"E-mail: {contato['email']}")
            st.write(f"Telefone: {contato['telefone']}")
            st.write(f"Data de registro: {contato['data']}")
            st.write('---')
    else:
        st.warning('Não há contatos registrados na agenda.')


# Lista que irá armazenar os contatos
contatos = list()


# Função principal que controla o fluxo do programa
def main():
    st.title('Programa Agenda')

    escolha = st.selectbox('Escolha uma opção:', (
    'Adicionar contato', 'Alterar contato', 'Procurar contato', 'Remover contato', 'Ver contatos'))

    if escolha == 'Adicionar contato':
        adicionar_contato()

    elif escolha == 'Alterar contato':
        alterar_contato()

    elif escolha == 'Procurar contato':
        procurar_contato()

    elif escolha == 'Remover contato':
        remover_contato()

    elif escolha == 'Ver contatos':
        ver_contatos()


if __name__ == '__main__':
    main()

