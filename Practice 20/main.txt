# ==========================================
# БАЗОВЫЙ КЛАСС
# ==========================================
# Персонал — общий класс для всех сотрудников.
# Здесь определены общие свойства и методы.
# Каждый наследник добавляет свои атрибуты и переопределяет методы.


class Personnel: # Базовый класс персонал

    def __init__(self, name, age, department): # Конструктор класса
        self.name = name # имя сотрудника
        self.age = age # возраст сотрудника
        self.department = department # отдел, где работает сотрудник

    def get_info(self): # метод, возвращающий общую информацию о сотруднике
        return f"{self.name}, {self.age} лет, отдел: {self.department}"

    def get_type(self): # Тип объекта (по умолчанию — 'Персонал')
        return "Персонал"

    def __str__(self): # Краткое представление объекта (для списка)
        return f"{self.get_type()}: {self.name}, {self.age} лет, отдел {self.department}"

# Дочерние классы

class HRPersonnel(Personnel):  # Кадровый служащий

    def __init__(self, name, age, department, position, experience): # конструктор класса
        super().__init__(name, age, department) # вызов конструктора базового класса для инициализации общих атрибутов
        self.position = position # должность
        self.experience = experience # стаж работы

    def get_info(self): # Переопределение метода (полиморфизм)
        return (f"Кадровый служащий: {self.name}\n"
                f"Возраст: {self.age}\n"
                f"Отдел: {self.department}\n"
                f"Должность: {self.position}\n"
                f"Стаж: {self.experience} лет\n")

    def get_type(self): # тип этого класса
        return "Кадровый служащий"


class Engineer(Personnel): # Инженер

    def __init__(self, name, age, department, specialty, projects): # конструктор инженера
        super().__init__(name, age, department) # инициализация общих атрибутов
        self.specialty = specialty # специальность инженера
        self.projects = projects # количество проектов

    def get_info(self):
        return (f"Инженер: {self.name}\n"
                f"Возраст: {self.age}\n"
                f"Отдел: {self.department}\n"
                f"Специальность: {self.specialty}\n"
                f"Количество проектов: {self.projects}\n")

    def get_type(self): # возвращает тип
        return "Инженер"


class AdminStaff(Personnel): # Административный персонал

    def __init__(self, name, age, department, role, work_hours): # конструктор
        super().__init__(name, age, department)
        self.role = role # роль
        self.work_hours = work_hours # количество рабочих часов в неделю

    def get_info(self):
        return (f"Административный персонал: {self.name}\n"
                f"Возраст: {self.age}\n"
                f"Отдел: {self.department}\n"
                f"Роль: {self.role}\n"
                f"Рабочих часов в неделю: {self.work_hours}\n")

    def get_type(self):
        return "Административный персонал"

# Композиция - класс реестр

class Registry: # Реестр сотрудников компании

    def __init__(self): # конструктор реестра
        self.staff_list = []

    def add_person(self, person): # Добавить сотрудника в список
        self.staff_list.append(person)
        print(f"{person.get_type()} '{person.name}' успешно добавлен!\n")

    def show_all(self): # Показать краткий список всех сотрудников
        if not self.staff_list: # если список пуст
            print("Реестр пуст.\n")
            return
        print("Список сотрудников:")
        for i, person in enumerate(self.staff_list, 1): # перебор сотрудников с нумерацией
            print(f"{i}. {person}") # номер и краткое представление

    def show_detailed(self): # Показать подробную информацию (демонстрация полиморфизма)
        if not self.staff_list: # Проверка на пустоту списка
            print("Реестр пуст.\n")
            return
        print("Подробная информация о сотрудниках:\n")
        for person in self.staff_list: # перебираем всех сотрудников
            print(person.get_info()) # проявляется полиморфизм, каждый класс вернет свою реализацию

# Меню

def main():
    registry = Registry() # экземпляр реестра

    while True:
        print("=== РЕЕСТР ПЕРСОНАЛА ===")
        print("1. Добавить кадрового служащего")
        print("2. Добавить инженера")
        print("3. Добавить административный персонал")
        print("4. Просмотреть краткий список")
        print("5. Просмотреть подробную информацию")
        print("0. Выход")

        choice = input("Выберите пункт меню: ")

        try: # для перехвата возможных ошибок
            if choice == "1":  #1. Добавить кадрового служащего
                name = input("ФИО: ")
                age = int(input("Возраст: "))
                dept = input("Отдел: ")
                position = input("Должность: ")
                exp = int(input("Стаж (лет): "))
                registry.add_person(HRPersonnel(name, age, dept, position, exp))

            elif choice == "2": # 2. Добавить инженера
                name = input("ФИО: ")
                age = int(input("Возраст: "))
                dept = input("Отдел: ")
                spec = input("Специальность: ")
                projects = int(input("Количество проектов: "))
                registry.add_person(Engineer(name, age, dept, spec, projects))

            elif choice == "3": # 3. Добавить административный персонал
                name = input("ФИО: ")
                age = int(input("Возраст: "))
                dept = input("Отдел: ")
                role = input("Роль: ")
                hours = int(input("Рабочих часов в неделю: "))
                registry.add_person(AdminStaff(name, age, dept, role, hours))

            elif choice == "4": # 4. Просмотреть краткий список
                registry.show_all()

            elif choice == "5": # 5. Просмотреть подробную информацию
                registry.show_detailed()

            elif choice == "0": # 0. Выход
                print("Выход из программы.")
                break

            else:
                print("Неверный пункт меню, попробуйте снова.\n")

        except ValueError:
            print("Ошибка ввода! Пожалуйста, введите корректные данные.\n")
        except Exception as e:
            print(f"Произошла ошибка: {e}\n")

# Запуск программы

if __name__ == "__main__":
    main() # запускаем главную функцию
