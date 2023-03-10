# 如何消除 Rspec 和 Capybara 规格中的重复代码

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/eliminate-duplicated-code-rspec-capybara>

 当我们的测试套件增长时，我们开始注意到代码的某些部分被复制了。重复的代码降低了可读性，并且难以管理，因为最终的更改或修复必须传播到所有重复的部分。解决这种情况的方法是保持你的代码干燥，意思是“不要重复你自己”。这是最基本的编程原则之一，我们在为我们的应用程序编写规范时也应该使用。

下面我将向你展示一些例子，告诉你如何干燥你的代码，去掉重复的代码。

## **将重复行放在块**之前

代码改进

```
context 'random play' do
  scenario 'plays random song with specific genre' do
    create :artist_with_picture
    app.home.load
    app.home.player.random_settings.click
    app.home.wait_for_random_play
    app.home.random_play.select_genre_by_name 'Rock'
    expect(app.home.artist_modal).to have_content 'Rock'
  end

  scenario 'plays random song with specific country' do
    create :artist_with_picture
    app.home.load
    app.home.player.random_settings.click
    app.home.wait_for_random_play
    app.home.random_play.select_country_by_name 'Poland'
    expect(app.home.artist_modal).to have_content 'Poland'
  end
end 
```

在这两个规格中，我们可以看到前四行是重复的。这应该给我们一个信号，我们应该使这个代码干燥。

干码

```
context 'random play' do
  before do
    create :artist_with_picture
    app.home.load
    app.home.player.random_settings.click
    app.home.wait_for_random_play
  end

  scenario 'plays random song with specific genre' do
    app.home.random_play.select_genre_by_name 'Rock'
    expect(app.home.artist_modal).to have_content 'Rock'
  end

  scenario 'plays random song with specific country' do
    app.home.random_play.select_country_by_name 'Poland'
    expect(app.home.artist_modal).to have_content 'Poland'
  end
end 
```

我们可以将这四行移到 before 块。由于这一点，块中的命令将在开始时执行，在每个场景之前，从而使我们的测试干燥和可读。

## **将重复行放到方法中**

代码改进

```
context 'when Admin is working on his skills' do
  before { skills_page.load }

  scenario 'he can add new skill' do
    skills_page.add_new_skill.click

    skills_page.skill_name.set 'capybara'
    skills_page.skill_description.set 'test test test'
    skills_page.skill_rate_type.select 'range'
    skills_page.skill_category.select 'backend'
    skills_page.requester_explanation.set 'test test test'
    skills_page.create_skill.click

    expect(draft_skills_page).to have_content 'New skill'
  end

  scenario 'he can edit skill' do
    skills_page.edit_skill.first.click

    skills_page.skill_name.set 'capybara'
    skills_page.skill_description.set 'test test test'
    skills_page.skill_rate_type.select 'range'
    skills_page.skill_category.select 'backend'
    skills_page.requester_explanation.set 'test test test'
    skills_page.create_skill.click

    expect(draft_skills_page).to have_content 'Edited skill'
  end
end 
```

在这种情况下，我们可以在规格中间看到相同的线条。因为规范的前几行互不相同，所以我们不能使用 before 块。

干码

```
context 'when Admin is working on his skills' do
  before { skills_page.load }

  def skill_form(page)
    page.skill_name.set 'capybara'
    page.skill_description.set 'test test test'
    page.skill_rate_type.select 'range'
    page.skill_category.select 'backend'
    page.requester_explanation.set 'test test test'
    page.create_skill.click
  end

  scenario 'he can add new skill' do
    skills_page.add_new_skill.click
    skill_form(skills_new_page)
    expect(draft_skills_page).to have_content 'New skill'
  end

  scenario 'he can edit skill' do
    skills_page.edit_skill.first.click
    skill_form(skill_edit_page)
    expect(draft_skills_page).to have_content 'Edited skill'
  end
end 
```

我们创建了一个包含所有重复代码的方法，这样以后我们可以在测试中引用这个方法。在这种情况下，我们还提供 page object 作为方法的参数。

## **预期从变为**

代码改进

```
scenario 'checks number of teams' do
  expect(Team.count).to eq 4
  teams_page.new_team_name_input.set('teamX')
  teams_page.save_team_button.click
  expect(Team.count).to eq 5
end 
```

在规范中，我们检查页面上的元素数量在某个动作之后是如何变化的。我们看到重复的行只有在达到预期的数目时才不同。

干码

```
scenario 'checks number of teams' do
  teams_page.new_team_name_input.set('teamX')

  expect { teams_page.save_team_button.click } # 2nd
    .to change { Team.count } # 1st, 3rd
    .from(4)
    .to(5)
end 
```

要修复这种重复，我们可以使用“期望从更改为”的方法。在这种情况下，我们检查页面上元素的数量，然后我们 **期望** 单击页面上的按钮将 **改变** 元素的数量 **从** x **到** y。因此，动作不是按时间顺序执行，而是按屏幕上可见的顺序执行——首先评估 **变化** **程序块** 以检查初始值，然后触发 **预期动作** ，最后再次评估 **变化** **程序块** 以查看 **是否来自** 和

## **预计在**之前发生变化

代码改进

```
scenario "User unlikes songs" do
  tracks_before = playlist.tracks.count
  app.home.playlist.tracks.first.unlike
  tracks_after = playlist.tracks.count
  expect(tracks_after).to eq(tracks_before - 1)
end 
```

在这种情况下，在开始时，我们保存页面上的元素数量，我们删除一个元素，然后在操作后检查元素的数量。最后，我们期望动作之后页面上的元素数量比动作之前少一个。这个规格还不错，但我们可以改进。

干码

```
scenario "User unlikes songs" do
  app.home.open_playlist

  expect { app.home.playlist.tracks.first.unlike }
    .to change { playlist.tracks.count }
    .by(-1)
end 
```

我们 **期望** 删除页面上的一个元素， **改变****元素的数量由** -1。

期望从改变到和期望改变到有什么区别？

在第一个函数中，我们感兴趣的是页面上元素的初始数量，而在第二个函数中，我们只对给定数量的变化感兴趣(初始状态不感兴趣)。

##  **场景中的循环——每个**

代码改进

```
context 'when user is admin' do
  scenario 'he sees all pages in the navbar' do
    expect(page.menu).to have_content 'Schedule'
    expect(page.menu).to have_content 'Users'
    expect(page.menu).to have_content 'Other'
    expect(page.menu).to have_content 'Project info'
  end
end 
```

这是一种典型的情况，我们检查页面上仅名称不同的元素的显示。单独检查每个元素会导致许多重复。

干码

```
context 'when user is admin' do
  scenario 'he sees all pages in the navbar' do
    links = ['Schedule', 'Users', 'Other', 'Project info']
    links.each do |link|
      expect(page).to have_content link
    end
  end
end 
```

我们创建一个包含重复元素名称的数组。我们调用数组上的每个循环**，这样数组的每个元素都成为我们的自变量。在循环中，我们检查给定的元素是否出现在屏幕上。**

 **另一种方式初始化一个数组

```
context 'when user is admin' do
  scenario 'he sees all pages in the navbar' do
    [
      'Schedule',
      'Users',
      'Other',
      'Project info'
    ].each do |link|
      expect(page).to have_content link
    end
  end
end 
```

使用%w 符号创建数组。

 **场景中的循环次数**

代码改进

```
 scenario 'creates empty list on the band page' do
    band_page.create_empty_list.next_button.click
    wait_for_ajax
    band_page.create_empty_list.next_button.click
    wait_for_ajax
    band_page.create_empty_list.next_button.click
    wait_for_ajax
    band_page.create_empty_list.next_button.click
    wait_for_ajax
    expect(page).to have_content 'Success!'
  end 
```

在这个规范中，我们想要测试一个空歌曲列表的创建。为此，我们将通过单击 4 个后续表单页面上的“下一步”按钮来浏览歌曲列表表单，而无需与表单本身进行交互。这就造成了大量的重复。

干码

```
scenario 'creates empty list on the band page' do
  4.times do
    band_page.create_empty_list.next_button.click
    wait_for_ajax
  end
  expect(page).to have_content 'Success!'
end 
```

**次数** 循环允许我们轻松地执行相同动作设定的次数。我们使用了 **times** 循环，而不是 **each** 循环，因为我们没有想要操作的集合，我们的操作也不需要参数化——我们只想简单地重复一个普通的操作，比如点击按钮 N 次。

## **围绕场景的循环**

代码改进

```
scenario "logs in user with role admin" do
  user = create(:admin)
  log_in(user)
  expect(user).to be_logged_in
end

scenario "logs in user with role student" do
  user = create(:student)
  log_in(user)
  expect(user).to be_logged_in
end 
```

我们有两个相同的场景，只是用户类型不同。

干码

```
roles = %w(admin student)
roles.each do |role|
  scenarios "logs in user with role #{role}" do
    user = create(role)
    log_in(user)
    expect(user).to be_logged_in
  end
end 
```

我们还可以围绕整个场景进行循环。在数组中，我们设置了在创建给定用户时引用的用户列表。在场景的标题上，我们使用插值来区分场景。还要注意，我们放入数组中的用户角色与我们正在使用的工厂的名称相匹配——这对于能够在每个场景中创建不同的用户至关重要。

## **页面对象中的循环**

代码改进

```
class PageObject < SitePrism::PageObject
  element :eyes, 'input[name="user[face_attributes][eyes]”]'
  element :nose, 'input[name="user[face_attributes][nose]”]'
  element :lips, 'input[name="user[face_attributes][lips]”]'
  element :mustache, 'input[name="user[face_attributes][mustache]”]'
end 
```

这个规范的问题在于页面对象元素的区别仅仅在于选择器中的名称。

 干码

```
class PageObject < SitePrism::PageObject
  [
    :eyes,
    :nose,
    :lips,
    :mustache
  ].each do |attribute|
    element attribute, "input[name=\"user[face_attributes][#{attribute}]\”]"
  end
end 
```

在添加页面对象时，我们也可以使用循环。我们创建一个包含重复元素的数组。然后我们用一个参数来使用每个循环。在循环中，我们使用数组中的参数定义每个元素——每个参数作为元素名插入，并插入到选择器中。

##  **创建 _ 列表**

代码改进

```
 context 'when user is a leader' do
  let(:lider) { create :user, :leader }
  let(:user_1) { create(:user) }
  let(:user_2) { create(:user) }
  let(:user_3) { create(:user) }
  let(:user_4) { create(:user) }

  scenario 'he sees his team members' do
    expect(page.team.members).to have_content user_1.full_name
    expect(page.team.members).to have_content user_2.full_name
    expect(page.team.members).to have_content user_3.full_name
    expect(page.team.members).to have_content user_4.full_name
  end
end 
```

我们分别创建每个用户，导致大量重复。

干码

```
context 'when user is a leader' do
  let(:lider) { create :user, :leader }
  let(:users) { create_list(:user, 4) }

  scenario 'he sees his team members' do
    users.each do |user|
      expect(page.team.members).to have_content user.full_name
    end
  end
end 
```

create_list 是一个工厂机器人(以前称为 factory girl)方法，它创建特定数量的用户，并以数组的形式返回这些用户。然后，我们再次使用每个循环来检查所有用户是否正确显示在页面上。

##  **分享例子**

代码改进

```
describe 'logging process WITH ARGUMENT' do
  let(:admin) { create(:admin) }
  let(:student) { create(:student) }
  let(:moderator) { create(:moderator) }

  context 'for admin' do
    scenario 'logs admin user' do
      login_page.email.set admin.email
      login_page.password.set admin.password
      login_page.login_button.click
      expect(page).to have_content admin.name
    end
  end

  context 'for student' do
    scenario 'logs student' do
      login_page.email.set student.email
      login_page.password.set student.password
      login_page.login_button.click
      expect(page).to have_content student.name
    end
  end

  context 'for moderator' do
    scenario 'logs moderator' do
      login_page.email.set moderator.email
      login_page.password.set moderator.password
      login_page.login_button.click
      expect(page).to have_content moderator.name
    end
  end
end 
```

我们有几个非常相似的测试，只是用户类型、电子邮件和密码不同。

干码

```
shared_examples 'user log in' do
  scenario 'properly logs in' do
    login_page.email.set user.email
    login_page.password.set user.password
    login_page.login_button.click
    expect(page).to have_user_name
  end
end

describe 'logging process WITH ARGUMENT' do
  context 'for admin' do
    let(:user) { create(:admin) }

    it_behaves_like 'user log in'
  end

  context 'for student' do
    let(:user) { create(:student) }

    it_behaves_like 'user log in'
  end

  context 'for moderator' do
    let(:user) { create(:moderator) }

    it_behaves_like 'user log in'
  end
end 
```

当你有多个描述相似行为的规范时，最好将它们提取出来，作为共享的例子。我们在我们的规范中包含了使用这些函数之一的共享示例:

*   include_examples "name" -包含当前上下文中的示例
*   it _ behaviors _ like“name”——在嵌套上下文中包含示例
*   it _ should _ behave _ like“name”——在嵌套上下文中包含示例
*   匹配元数据-包括当前上下文中的示例

在我们的规范中，我们用变量 **用户** 创建共享示例，然后在每个规范中，我们调用共享示例并提供一个用户作为其参数。这样，共享示例中的用户变量将被设置为我们提供给给定共享示例的用户。

```
shared_examples 'user log in' do |user|
  scenario 'properly logs in' do
    login_page.email.set user.email
    login_page.password.set user.password
    login_page.login_button.click
    expect(page).to have_user_name
  end
end

describe 'logging process WITH ARGUMENT' do
  let(:admin) { create(:admin) }
  let(:student) { create(:student) }
  let(:moderator) { create(:moderator) }

  context 'for admin' do
    it_behaves_like 'user log in', admin
  end

  context 'for student' do
    it_behaves_like 'user log in', student
  end

  context 'for moderator' do
    it_behaves_like 'user log in', moderator
  end
end 
```

我们也可以先通过 **让** 创建用户，然后通过参数指明哪个 使用我们想要检查的 。

##  **泼溅操作员**

代码改进

```
describe 'Main page' do
  context 'when there are several users on the page' do
    let(:user1) do
      create(
        :user,
        name: 'Woody',
        surname: 'Allen',
        age: '90',
        address: {
          city: 'New York',
          country: 'USA',
          postal_code: '78-278'
        },
        nationality: 'American',
      )
    end

    let(:user2) do
      create(
        :user,
        name: 'Johny',
        surname: 'Bravo',
        age: '90',
        address: {
          city: 'New York',
          country: 'USA',
          postal_code: '78-278'
        },
        nationality: 'American',
      )
    end

    let(:user3) do
      create(
        :user,
        name: 'Jessica',
        surname: 'Alba',
        age: '90',
        address: {
          city: 'New York',
          country: 'USA',
          postal_code: '78-278'
        },
        nationality: 'American',
      )
    end
  end 
```

我们创建了几个用户，将特定的长期数据存储在哈希中。一部分用户数据对于每个用户都是相同的。

干码

```
context 'when there are several users on the page' do
    let(:common_params) do
      {
        age: '90',
        address: {
          city: 'New York',
          country: 'USA',
          postal_code: '78-278'
        },
        nationality: 'American',
      }
    end

    let(:user1) { create(:user, name: 'Woody', surname: 'Allen', **common_params) }
    let(:user2) { create(:user, name: 'Johny', surname: 'Bravo', **common_params) }
    let(:user3) { create(:user, name: 'Jessica', surname: 'Alba', **common_params) }
  end
end 
```

在我们的规范中，我们创建了一个**字母**，其中的重复数据存储在 hash 中。然后，我们分别创建每个用户，并使用带有双 splat 操作符的公共数据。感谢 double splat 操作符，我们的重复数据将被“解包”并提供给每个方法，就像其余的参数一样。

以上例子只是对改进代码的建议。使用这些技术并不总是能增加代码的可读性，你必须小心行事。但我认为，在大多数情况下，他们会减少代码量，使其更容易维护。**