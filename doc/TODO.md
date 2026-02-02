# TODO

Suite à un audit effectué en amont, voici les failles et les bugs qui ont été identifés comme prioritaire.

## FAILLES

* Des utilsateurs non admin ont des accès à l'interface de gestion des utilisateurs
* Les mots de passes ne sont pas chiffrée en base de données...
* Des injections de type XSS ont été détéctées sur certains formulaires
* On nous a signalé des injections SQL lors de la création d'une nouvelles habitudes
  * exemple dans le champs "name" : foo', 'INJECTED-DESC', NOW()); --

## BUGS

* Une 404 est détéctée lors de la redirection après l'ajout d'une habitude
* Le formulaire d'inscription ne semble pas fonctionner
* Fatal error: Uncaught Error: Class "App\Controller\Api\HabitsController" lorsque l'on accède à l'URL  ``/api/habits``

**ATTENTION : certains bugs n'ont pas été listé**

## REPONSE
## 1. Broken Access Control (A01:2021)
- `config/routes.json:29` → `/admin/user` sans `guard`
- `config/routes.json:34` → `/admin/user/new` sans `guard`
- `config/routes.json:45` → `/habits` sans `guard`
- `config/routes.json:50` → `/habits/create` sans `guard`
- `config/routes.json:55` → `/api/habits` sans `guard`
- `src/Controller/Member/HabitsController.php:71-72` + `src/Repository/HabitLogRepository.php:89-106` -> IDOR (toggle sans contrôle propriétaire)

## 2. Injection SQL (A03:2021)
- `src/Repository/HabitRepository.php:18` → `WHERE id = $id`
- `src/Repository/HabitRepository.php:24` → `WHERE user_id = $userId`
- `src/Repository/HabitRepository.php:46` → `INSERT ... VALUES (" . $data['user_id'] . ", '" . $name . "', '" . $description . "', NOW())`
- `src/Repository/HabitLogRepository.php:27` → `WHERE habit_id = $habitId`
- `src/Repository/UserRepository.php:18` → `WHERE id = $id`
- `src/Repository/UserRepository.php:24` → `WHERE email = '$email'`

## 3. XSS (A03:2021)
- `templates/member/dashboard/index.html.php:4` → `<?= $_SESSION['user']['firstname'] ?>`
- `templates/member/dashboard/index.html.php:50-51` → `<?= $habit->getName() ?>` / `<?= $habit->getDescription() ?>`
- `templates/admin/user/index.html.php:28-31` → id / prénom / nom / email
- `templates/admin/partials/_sidebar.html.php:24-25` → username
- `templates/shared/error.html.php:8` → `<?= $message ?>`
- `templates/security/login.html.php:11` → `<?= $error ?>`
- `templates/register/index.html.php:11` → `<?= $error ?>`
- `templates/admin/user/new.html.php:17` → `<?= $error ?>`

## 4. Mots de passe en clair (A02:2021)
- `database.sql:24-29` → `azertyuiop`
- `demo_data.sql:10`
- `src/Controller/SecurityController.php:33` → `if($password == $user->getPassword())`
- `src/Repository/UserRepository.php:31` → insert sans hash

## 5. CSRF (A01:2021)
- `templates/member/habits/new.html.php`
- `templates/member/dashboard/index.html.php` (toggle)
- `templates/admin/user/new.html.php`
- `templates/register/index.html.php`
