<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Умный помощник ТСЖ - Панель управления</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .tab {
            transition: all 0.3s ease;
            white-space: nowrap;
            font-size: 0.9rem;
        }
        .tab:hover {
            background-color: #E5E7EB;
        }
        .shadow-card {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        #supportChatButton {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background-color: #1D4ED8;
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.2s;
        }
        #supportChatButton:hover {
            transform: scale(1.1);
        }
    </style>
</head>
<body class="bg-gray-100 text-gray-900">
    <!-- Header -->
    <header class="bg-blue-700 text-white p-4 flex justify-between items-center">
        <h1 class="text-2xl font-bold">Умный помощник ТСЖ</h1>
        <button id="logoutButton" class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg transition">Выйти</button>
    </header>

    <!-- Main Content -->
    <div class="container mx-auto p-6">
        <!-- Navigation Tabs -->
        <div class="flex border-b border-gray-200 mb-6 overflow-x-auto">
            <button id="appealsTab" class="tab px-3 py-2 text-blue-700 border-b-2 border-blue-700 font-semibold">Обращения</button>
            <button id="templatesTab" class="tab px-3 py-2 text-gray-600 font-semibold">Шаблоны</button>
            <button id="notificationsTab" class="tab px-3 py-2 text-gray-600 font-semibold">Уведомления</button>
            <button id="analyticsTab" class="tab px-3 py-2 text-gray-600 font-semibold">Аналитика</button>
            <button id="aiAssistantTab" class="tab px-3 py-2 text-gray-600 font-semibold">ИИ</button>
            <button id="employeesTab" class="tab px-3 py-2 text-gray-600 font-semibold">Сотрудники</button>
            <button id="emergencyContactsTab" class="tab px-3 py-2 text-gray-600 font-semibold">Экстренные</button>
        </div>

        <!-- Appeals Section -->
        <section id="appeals" class="tab-content">
            <h2 class="text-xl font-semibold mb-4">Обращения жильцов</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <!-- Filters -->
                <div class="mb-4 flex space-x-4">
                    <select id="statusFilter" class="border border-gray-300 p-2 rounded-lg">
                        <option value="">Все статусы</option>
                        <option value="new">Новое</option>
                        <option value="in_progress">В работе</option>
                        <option value="resolved">Решено</option>
                    </select>
                    <input type="date" id="dateFilter" class="border border-gray-300 p-2 rounded-lg">
                    <select id="typeFilter" class="border border-gray-300 p-2 rounded-lg">
                        <option value="">Все типы</option>
                        <option value="water">Отключение воды</option>
                        <option value="payment">Оплата</option>
                        <option value="other">Прочее</option>
                    </select>
                </div>
                <!-- Appeals Table -->
                <table class="w-full text-left">
                    <thead>
                        <tr class="bg-gray-50">
                            <th class="p-3">ID</th>
                            <th class="p-3">Жилец</th>
                            <th class="p-3">Тип</th>
                            <th class="p-3">Статус</th>
                            <th class="p-3">Дата</th>
                            <th class="p-3">Действия</th>
                        </tr>
                    </thead>
                    <tbody id="appealsList">
                        <tr data-id="1">
                            <td class="p-3">1</td>
                            <td class="p-3">Иванов И.И.</td>
                            <td class="p-3">Отключение воды</td>
                            <td class="p-3">Новое</td>
                            <td class="p-3">2025-08-04</td>
                            <td class="p-3">
                                <button class="viewAppeal bg-blue-700 hover:bg-blue-800 text-white py-1 px-3 rounded-lg">Просмотр</button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <!-- Appeal Details Modal -->
            <div id="appealModal" class="hidden fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center">
                <div class="bg-white p-6 rounded-lg shadow-card w-1/2">
                    <h3 class="text-lg font-semibold mb-4">Детали обращения</h3>
                    <p><strong>Жилец:</strong> <span id="appealResident"></span></p>
                    <p><strong>Тип:</strong> <span id="appealType"></span></p>
                    <p><strong>Описание:</strong> <span id="appealDescription">Проблема с водой в квартире</span></p>
                    <p><strong>Файлы:</strong> <a href="#" class="text-blue-700 hover:underline">photo.jpg</a></p>
                    <p><strong>История:</strong></p>
                    <ul id="appealHistory" class="list-disc pl-5 mb-4">
                        <li>2025-08-04: Обращение создано</li>
                    </ul>
                    <div class="mb-4">
                        <label class="block text-sm font-medium">Статус:</label>
                        <select id="appealStatus" class="border border-gray-300 p-2 w-full rounded-lg">
                            <option value="new">Новое</option>
                            <option value="in_progress">В работе</option>
                            <option value="resolved">Решено</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-sm font-medium">Исполнитель:</label>
                        <select id="appealAssignee" class="border border-gray-300 p-2 w-full rounded-lg">
                            <option value="">Не назначен</option>
                            <option value="petrov">Петров П.П.</option>
                            <option value="sidorov">Сидоров С.С.</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-sm font-medium">Ответ жильцу:</label>
                        <textarea id="appealResponse" placeholder="Введите ответ" class="border border-gray-300 p-2 w-full rounded-lg h-24"></textarea>
                    </div>
                    <div class="flex space-x-2">
                        <button id="saveAppeal" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Сохранить</button>
                        <button id="closeModal" class="bg-gray-500 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-lg">Закрыть</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- Templates Section -->
        <section id="templates" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">Шаблоны документов</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <button id="addTemplate" class="bg-amber-500 hover:bg-amber-600 text-white font-semibold py-2 px-4 rounded-lg mb-4">Добавить шаблон</button>
                <div id="templateForm" class="hidden mb-4">
                    <input type="text" id="templateName" placeholder="Название шаблона" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <textarea id="templateContent" placeholder="Текст шаблона" class="border border-gray-300 p-2 mb-2 w-full h-32 rounded-lg"></textarea>
                    <button id="saveTemplate" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Сохранить</button>
                </div>
                <ul id="templateList" class="list-disc pl-5">
                    <li>Уведомление о задолженности <button class="text-blue-700 hover:underline">Редактировать</button></li>
                </ul>
            </div>
        </section>

        <!-- Notifications Section -->
        <section id="notifications" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">Уведомления</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <button id="addNotification" class="bg-amber-500 hover:bg-amber-600 text-white font-semibold py-2 px-4 rounded-lg mb-4">Создать уведомление</button>
                <div id="notificationForm" class="hidden mb-4">
                    <select id="notificationType" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                        <option>Напоминание о задолженности</option>
                        <option>Отключение воды</option>
                    </select>
                    <input type="text" id="notificationRecipient" placeholder="Получатели" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <button id="sendNotification" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Отправить</button>
                </div>
                <table class="w-full text-left">
                    <thead>
                        <tr class="bg-gray-50">
                            <th class="p-3">Тип</th>
                            <th class="p-3">Получатель</th>
                            <th class="p-3">Статус</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td class="p-3">Задолженность</td>
                            <td class="p-3">Иванов И.И.</td>
                            <td class="p-3">Отправлено</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <!-- Analytics Section -->
        <section id="analytics" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">Аналитика</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <button id="generateReport" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg mb-4">Сгенерировать отчет</button>
                <h3 class="text-lg font-medium mb-2">Статистика обращений</h3>
                <p>Всего обращений: 10</p>
                <p>Типовые вопросы: 8</p>
                <p>Нестандартные вопросы: 2</p>
                <h3 class="text-lg font-medium mb-2 mt-4">Задолженности</h3>
                <p>Общая сумма: 50,000 руб.</p>
                <p>Должников: 5</p>
            </div>
        </section>

        <!-- AI Assistant Section -->
        <section id="aiAssistant" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">ИИ-помощник</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <label class="block text-sm font-medium mb-1">Задайте вопрос ИИ</label>
                <input type="text" id="aiQuestion" placeholder="Например: 'Как ответить на жалобу о воде?'" class="border border-gray-300 p-2 w-full rounded-lg mb-4">
                <button id="askAI" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Получить ответ</button>
                <div id="aiResponse" class="mt-4 text-gray-900"></div>
            </div>
        </section>

        <!-- Employees Section -->
        <section id="employees" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">Сотрудники УК</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <button id="addEmployee" class="bg-amber-500 hover:bg-amber-600 text-white font-semibold py-2 px-4 rounded-lg mb-4">Добавить сотрудника</button>
                <div id="employeeForm" class="hidden mb-4">
                    <input type="text" id="employeeName" placeholder="ФИО" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <select id="employeeRole" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                        <option value="electrician">Электрик</option>
                        <option value="accountant">Бухгалтер</option>
                        <option value="director">Директор</option>
                        <option value="manager">Менеджер</option>
                    </select>
                    <input type="text" id="employeePhone" placeholder="Телефон" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <input type="email" id="employeeEmail" placeholder="Email" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <button id="saveEmployee" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Сохранить</button>
                </div>
                <table class="w-full text-left">
                    <thead>
                        <tr class="bg-gray-50">
                            <th class="p-3">ФИО</th>
                            <th class="p-3">Должность</th>
                            <th class="p-3">Телефон</th>
                            <th class="p-3">Email</th>
                        </tr>
                    </thead>
                    <tbody id="employeeList">
                        <tr>
                            <td class="p-3">Петров П.П.</td>
                            <td class="p-3">Электрик</td>
                            <td class="p-3">+7 (861) 987-65-43</td>
                            <td class="p-3">petrov@example.com</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <!-- Emergency Contacts Section -->
        <section id="emergencyContacts" class="tab-content hidden">
            <h2 class="text-xl font-semibold mb-4">Экстренные контакты</h2>
            <div class="bg-white p-6 rounded-lg shadow-card">
                <button id="addEmergencyContact" class="bg-amber-500 hover:bg-amber-600 text-white font-semibold py-2 px-4 rounded-lg mb-4">Добавить контакт</button>
                <div id="emergencyContactForm" class="hidden mb-4">
                    <input type="text" id="contactName" placeholder="Название (например, Аварийная служба)" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <input type="text" id="contactPhone" placeholder="Телефон" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <input type="email" id="contactEmail" placeholder="Email" class="border border-gray-300 p-2 mb-2 w-full rounded-lg">
                    <button id="saveEmergencyContact" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Сохранить</button>
                </div>
                <table class="w-full text-left">
                    <thead>
                        <tr class="bg-gray-50">
                            <th class="p-3">Название</th>
                            <th class="p-3">Телефон</th>
                            <th class="p-3">Email</th>
                        </tr>
                    </thead>
                    <tbody id="emergencyContactList">
                        <tr>
                            <td class="p-3">Аварийная служба</td>
                            <td class="p-3">+7 (861) 123-45-67</td>
                            <td class="p-3">emergency@example.com</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <!-- Support Chat Button -->
        <div id="supportChatButton" title="Связаться с поддержкой">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M9 16H5v-4l7-7 4 4 7-7v4h-4m-7 7h4" />
            </svg>
        </div>

        <!-- Support Chat Modal -->
        <div id="supportChatModal" class="hidden fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center">
            <div class="bg-white p-6 rounded-lg shadow-card w-96">
                <h3 class="text-lg font-semibold mb-4">Связь с поддержкой</h3>
                <textarea id="supportMessage" placeholder="Опишите вашу проблему" class="border border-gray-300 p-2 w-full rounded-lg h-32 mb-4"></textarea>
                <div class="flex space-x-2">
                    <button id="sendSupportMessage" class="bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2 px-4 rounded-lg">Отправить</button>
                    <button id="closeSupportModal" class="bg-gray-500 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-lg">Закрыть</button>
                </div>
            </div>
        </div>
    </div>

    <!-- JavaScript for Functionality -->
    <script>
        // Tab switching
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');

        tabs.forEach(tab => {
            tab.addEventListener('click', () => {
                tabs.forEach(t => {
                    t.classList.remove('text-blue-700', 'border-b-2', 'border-blue-700');
                    t.classList.add('text-gray-600');
                });
                tab.classList.add('text-blue-700', 'border-b-2', 'border-blue-700');
                tabContents.forEach(content => content.classList.add('hidden'));
                document.getElementById(tab.id.replace('Tab', '')).classList.remove('hidden');
            });
        });

        // Template form toggle
        document.getElementById('addTemplate').addEventListener('click', () => {
            document.getElementById('templateForm').classList.toggle('hidden');
        });

        // Notification form toggle
        document.getElementById('addNotification').addEventListener('click', () => {
            document.getElementById('notificationForm').classList.toggle('hidden');
        });

        // Employee form toggle
        document.getElementById('addEmployee').addEventListener('click', () => {
            document.getElementById('employeeForm').classList.toggle('hidden');
        });

        // Emergency contact form toggle
        document.getElementById('addEmergencyContact').addEventListener('click', () => {
            document.getElementById('emergencyContactForm').classList.toggle('hidden');
        });

        // Support chat modal toggle
        document.getElementById('supportChatButton').addEventListener('click', () => {
            document.getElementById('supportChatModal').classList.toggle('hidden');
        });

        document.getElementById('closeSupportModal').addEventListener('click', () => {
            document.getElementById('supportChatModal').classList.add('hidden');
        });

        // Mock send support message
        document.getElementById('sendSupportMessage').addEventListener('click', () => {
            const message = document.getElementById('supportMessage').value;
            if (message) {
                alert('Сообщение отправлено: ' + message);
                document.getElementById('supportMessage').value = '';
                document.getElementById('supportChatModal').classList.add('hidden');
            } else {
                alert('Пожалуйста, введите сообщение.');
            }
        });

        // Mock save template
        document.getElementById('saveTemplate').addEventListener('click', () => {
            const name = document.getElementById('templateName').value;
            if (name) {
                const li = document.createElement('li');
                li.innerHTML = `${name} <button class="text-blue-700 hover:underline">Редактировать</button>`;
                document.getElementById('templateList').appendChild(li);
                document.getElementById('templateForm').classList.add('hidden');
            }
        });

        // Mock save employee
        document.getElementById('saveEmployee').addEventListener('click', () => {
            const name = document.getElementById('employeeName').value;
            const role = document.getElementById('employeeRole').value;
            const phone = document.getElementById('employeePhone').value;
            const email = document.getElementById('employeeEmail').value;
            if (name && role) {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="p-3">${name}</td>
                    <td class="p-3">${role === 'electrician' ? 'Электрик' : role === 'accountant' ? 'Бухгалтер' : role === 'director' ? 'Директор' : 'Менеджер'}</td>
                    <td class="p-3">${phone}</td>
                    <td class="p-3">${email}</td>
                `;
                document.getElementById('employeeList').appendChild(tr);
                document.getElementById('employeeForm').classList.add('hidden');
            }
        });

        // Mock save emergency contact
        document.getElementById('saveEmergencyContact').addEventListener('click', () => {
            const name = document.getElementById('contactName').value;
            const phone = document.getElementById('contactPhone').value;
            const email = document.getElementById('contactEmail').value;
            if (name) {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="p-3">${name}</td>
                    <td class="p-3">${phone}</td>
                    <td class="p-3">${email}</td>
                `;
                document.getElementById('emergencyContactList').appendChild(tr);
                document.getElementById('emergencyContactForm').classList.add('hidden');
            }
        });

        // Appeal modal handling
        document.querySelectorAll('.viewAppeal').forEach(button => {
            button.addEventListener('click', (e) => {
                const row = e.target.closest('tr');
                document.getElementById('appealResident').textContent = row.cells[1].textContent;
                document.getElementById('appealType').textContent = row.cells[2].textContent;
                document.getElementById('appealStatus').value = row.cells[3].textContent.toLowerCase().replace(' ', '_');
                document.getElementById('appealModal').classList.remove('hidden');
            });
        });

        document.getElementById('closeModal').addEventListener('click', () => {
            document.getElementById('appealModal').classList.add('hidden');
        });

        document.getElementById('saveAppeal').addEventListener('click', () => {
            const status = document.getElementById('appealStatus').value;
            const assignee = document.getElementById('appealAssignee').value;
            const response = document.getElementById('appealResponse').value;
            if (response) {
                const li = document.createElement('li');
                li.textContent = `${new Date().toLocaleDateString()}: ${response} (Исполнитель: ${assignee || 'не назначен'})`;
                document.getElementById('appealHistory').appendChild(li);
            }
            document.getElementById('appealModal').classList.add('hidden');
        });

        // Mock AI assistant
        document.getElementById('askAI').addEventListener('click', () => {
            const question = document.getElementById('aiQuestion').value;
            const aiResponse = document.getElementById('aiResponse');
            if (question) {
                // Имитация ответа ИИ (в реальной версии использовать OpenAI API)
                const mockResponses = {
                    'как ответить на жалобу': 'Рекомендуется извиниться за неудобства, уточнить детали проблемы и сообщить о сроках решения. Пример: "Уважаемый житель, приносим извинения. Специалист выедет завтра в 10:00."',
                    'отключение воды': 'Уточните адрес и сроки отключения, предложите альтернативные решения, например: "Вода будет отключена 5 августа с 9:00 до 14:00. Рекомендуем запастись водой."',
                    '': 'Пожалуйста, задайте конкретный вопрос.'
                };
                aiResponse.innerHTML = mockResponses[question.toLowerCase()] || 'ИИ: Не могу ответить, уточните вопрос.';
            } else {
                aiResponse.innerHTML = 'Пожалуйста, введите вопрос.';
            }
        });

        // Mock generate report
        document.getElementById('generateReport').addEventListener('click', () => {
            alert('Отчет сгенерирован (заглушка, требует бэкенда)');
        });

        // Mock logout
        document.getElementById('logoutButton').addEventListener('click', () => {
            alert('Выход из системы');
        });
    </script>
</body>
</html>
