# Coletando Dados do Linkedin com Selenium
Durante as aulas da Awari aprendi a utilizar bibliotecas como BeautifulSoup e Selenium para fazer realizar Web Scraping (raspar dados de sites da web). Um dos exercícios propostos é que você realize um procedimento de Web Scraping que retenha descrições das vagas abertas em ciência de dados no Linkedin.

### Código comentado:

    # Importações
    from selenium import webdriver
    from time import sleep

    # Parâmetros
    URL_LINKEDIN_DS = 'https://www.linkedin.com/jobs/ci%C3%AAncia-de-dados-vagas/?currentJobId=3229957342&originalSubdomain=br'



    # Funções e classes



    # Execução do código
    if __name__ == '__main__':
        # Criar uma instância do Google Chrome pelo Selenium
        driver = webdriver.Chrome()
        driver.implicitly_wait(10)

        # Acessar URL do Linkedin
        driver.get(URL_LINKEDIN_DS)

        # Pegar lista de resultado de vagas de CIência de Dados
        resultados = driver.find_elements_by_class_name('jobs-search-results-list')
        lista_descricao = []

        # Iniciar While loop em cima de todos os resultados
        while True:
            # For loop para coletar descrições de dados
            for r in resultados[len(lista_descricao):]:
                r.click()
                sleep(1)
                try:
                    # Pegar elemento com a descrição
                    descricao = driver.find_element_by_class_name('description')
                    # Anexar o texto na lista
                    lista_descricao.append(descricao.text)
                except:
                    print('Erro')
                    pass

            resultados = driver.find_elements_by_class_name('jobs-search-results-list')

            print(f'Número de resultados {len(resultados)}')
            print(f'Número de descrições que foram salvas {len(lista_descricao)}')

            # Critério de saída do While
            if len(lista_descricao) == len(resultados):
                break

        print(lista_descricao)
        # Salvar descrições
        descricao_salvar = '\n'.join(lista_descricao)
        with open('descricoes_vagas.txt', 'w') as f:
            f.write(descricao_salvar)

        # Fechar o Coogle Chrome
        driver.quit()
