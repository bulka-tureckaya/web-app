# web-app
Это решение представляет собой YAML-манифест для развертывания веб-приложения в Kubernetes. В процессе разработки я использовала официальную документацию Kubernetes и адаптировала примеры под требования задачи.

## Выбор компонентов и создание решения
Для решения задачи я выбрала следующие компоненты Kubernetes:
### Deployment
Я начала с создания Deployment. Взяла пример из документации и адаптировала его под требования задачи:
- Указала 4 реплики, так как это минимальное количество подов для пиковой нагрузки.

- Добавила *resources* для управления CPU и memory, чтобы приложение не превышало лимиты и получало гарантированные ресурсы.

- Добавила *readinessProbe*, чтобы Kubernetes не отправлял трафик на поды, пока они не будут готовы (учтено время инициализации 5-10 секунд).

### PDB
Чтобы приложение было отказоустойчивым, я добавила Pod Disruption Budget (PDB). Это гарантирует, что минимум 3 пода будут доступны всегда. Взяла пример из документации и адаптировала его:
- Указала *minAvailable: 3*, чтобы гарантировать доступность 3 подов из 4.

### HPA
Для экономии ресурсов я добавила Horizontal Pod Autoscaler (HPA). Он автоматически масштабирует количество подов в зависимости от нагрузки. Это важно, так как у приложения дневной цикл нагрузки (пик днем, минимум ночью). Взяла пример из документации и адаптировала его:
- Указала *minReplicas: 2* (ночью) и *maxReplicas: 4* (днем).

- Настроила масштабирование на основе CPU (*averageUtilization: 50*).
