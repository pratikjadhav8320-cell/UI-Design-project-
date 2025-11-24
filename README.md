import React, { useState } from "react";

export default function App() {
  const [route, setRoute] = useState("login"); // routes: login, dashboard, profile, settings
  const [user, setUser] = useState({ name: "Pratiksingh Jadhav", course: "BCA", email: "pratik@example.com" });
  const [auth, setAuth] = useState(false);

  // Mock login handler
  function handleLogin(e) {
    e.preventDefault();
    // simple auth simulation
    setAuth(true);
    setRoute("dashboard");
  }

  function handleLogout() {
    setAuth(false);
    setRoute("login");
  }

  return (
    <div className="min-h-screen bg-slate-50 font-sans text-slate-800">
      <header className="bg-white shadow-sm">
        <div className="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-full bg-gradient-to-br from-indigo-500 to-pink-500 flex items-center justify-center text-white font-bold">UI</div>
            <div>
              <h1 className="text-lg font-semibold">UI Designing Project</h1>
              <p className="text-xs text-slate-500">A simple responsive UI scaffold</p>
            </div>
          </div>

          <nav className="flex items-center gap-3">
            {auth ? (
              <>
                <button onClick={() => setRoute("dashboard")} className={navBtnClass(route === "dashboard")}>Dashboard</button>
                <button onClick={() => setRoute("profile")} className={navBtnClass(route === "profile")}>Profile</button>
                <button onClick={() => setRoute("settings")} className={navBtnClass(route === "settings")}>Settings</button>
                <button onClick={handleLogout} className="px-3 py-2 rounded-md text-sm bg-red-50 text-red-600">Logout</button>
              </>
            ) : (
              <button onClick={() => setRoute("login")} className={navBtnClass(route === "login")}>Login</button>
            )}
          </nav>
        </div>
      </header>

      <main className="max-w-6xl mx-auto px-4 py-8">
        {route === "login" && <LoginView onLogin={handleLogin} />}
        {route === "dashboard" && auth && <DashboardView user={user} onNavigate={setRoute} />}
        {route === "profile" && auth && <ProfileView user={user} setUser={setUser} />}
        {route === "settings" && auth && <SettingsView />}

        {!auth && route !== "login" && (
          <div className="mt-6 p-4 bg-yellow-50 border-l-4 border-yellow-300 text-yellow-800 rounded">Please login to access this page.</div>
        )}
      </main>

      <footer className="mt-12 py-6 bg-white border-t">
        <div className="max-w-6xl mx-auto px-4 text-sm text-slate-500">© {new Date().getFullYear()} UI Designing Project — Ajeenkya DY Patil University</div>
      </footer>
    </div>
  );
}

// ======== Helper Components & Functions ========

function navBtnClass(active) {
  return (
    `px-3 py-2 rounded-md text-sm ${active ? "bg-indigo-50 text-indigo-600 font-medium" : "text-slate-600 hover:bg-slate-100"}`
  );
}

function LoginView({ onLogin }) {
  return (
    <div className="max-w-md mx-auto mt-12">
      <div className="bg-white border rounded-lg shadow-sm p-6">
        <h2 className="text-2xl font-semibold mb-2">Welcome back</h2>
        <p className="text-sm text-slate-500 mb-6">Login to access your UI design dashboard</p>

        <form onSubmit={onLogin} className="space-y-4">
          <div>
            <label className="block text-sm font-medium text-slate-700">Email</label>
            <input required type="email" placeholder="you@example.com" className="mt-1 block w-full rounded-md border-slate-200 shadow-sm focus:ring-indigo-500 focus:border-indigo-500" />
          </div>

          <div>
            <label className="block text-sm font-medium text-slate-700">Password</label>
            <input required type="password" placeholder="••••••••" className="mt-1 block w-full rounded-md border-slate-200 shadow-sm focus:ring-indigo-500 focus:border-indigo-500" />
          </div>

          <div className="flex items-center justify-between">
            <div className="flex items-center gap-2">
              <input id="remember" type="checkbox" className="h-4 w-4 text-indigo-600" />
              <label htmlFor="remember" className="text-sm text-slate-600">Remember me</label>
            </div>
            <a href="#" className="text-sm text-indigo-600">Forgot?</a>
          </div>

          <div>
            <button type="submit" className="w-full py-2 rounded-md bg-indigo-600 text-white font-medium">Login</button>
          </div>
        </form>

        <div className="mt-4 text-center text-sm text-slate-500">Or continue with</div>
        <div className="mt-3 flex gap-2 justify-center">
          <button className="px-3 py-2 rounded-md border w-28">Google</button>
          <button className="px-3 py-2 rounded-md border w-28">GitHub</button>
        </div>
      </div>

      <div className="mt-6 text-center text-sm text-slate-500">Demo account — no backend required.</div>
    </div>
  );
}

function DashboardView({ user, onNavigate }) {
  // sample stats
  const stats = [
    { id: 1, title: "Projects", value: 5, delta: "+2" },
    { id: 2, title: "Tasks", value: 24, delta: "-1" },
    { id: 3, title: "Active Users", value: 120, delta: "+8" },
  ];

  const projects = [
    { id: 1, title: "College Portal UI", status: "In Progress" },
    { id: 2, title: "E-commerce Dashboard", status: "Planned" },
    { id: 3, title: "Mobile App UI", status: "Completed" },
  ];

  return (
    <div className="grid grid-cols-1 lg:grid-cols-4 gap-6">
      <aside className="lg:col-span-1 bg-white rounded-lg border p-4 shadow-sm">
        <div className="flex items-center gap-3">
          <div className="w-12 h-12 rounded-full bg-indigo-100 flex items-center justify-center text-indigo-700 font-semibold">P</div>
          <div>
            <div className="text-sm font-medium">{user.name}</div>
            <div className="text-xs text-slate-500">{user.course}</div>
          </div>
        </div>

        <nav className="mt-6 flex flex-col gap-2">
          <button onClick={() => onNavigate("dashboard")} className="text-left px-2 py-2 rounded hover:bg-slate-50">Overview</button>
          <button onClick={() => onNavigate("profile")} className="text-left px-2 py-2 rounded hover:bg-slate-50">Profile</button>
          <button onClick={() => onNavigate("settings")} className="text-left px-2 py-2 rounded hover:bg-slate-50">Settings</button>
        </nav>
      </aside>

      <section className="lg:col-span-3 space-y-6">
        <div className="bg-white p-4 rounded-lg border shadow-sm">
          <h2 className="text-lg font-semibold">Dashboard</h2>
          <p className="text-sm text-slate-500">Quick overview of your projects and stats</p>

          <div className="mt-4 grid grid-cols-1 sm:grid-cols-3 gap-4">
            {stats.map(s => (
              <div key={s.id} className="p-4 rounded-lg border">
                <div className="text-xs text-slate-500">{s.title}</div>
                <div className="text-2xl font-semibold">{s.value}</div>
                <div className="text-sm text-slate-400 mt-1">{s.delta} since last week</div>
              </div>
            ))}
          </div>
        </div>

        <div className="bg-white p-4 rounded-lg border shadow-sm">
          <div className="flex items-center justify-between">
            <h3 className="font-semibold">Projects</h3>
            <button className="text-sm text-indigo-600">New Project</button>
          </div>

          <ul className="mt-3 divide-y">
            {projects.map(p => (
              <li key={p.id} className="py-3 flex items-center justify-between">
                <div>
                  <div className="font-medium">{p.title}</div>
                  <div className="text-xs text-slate-500">{p.status}</div>
                </div>
                <div>
                  <button className="px-3 py-1 rounded bg-slate-100 text-sm">Open</button>
                </div>
              </li>
            ))}
          </ul>
        </div>

        <div className="bg-white p-4 rounded-lg border shadow-sm">
          <h3 className="font-semibold">Recent Activity</h3>
          <ol className="mt-3 list-decimal list-inside text-sm text-slate-600">
            <li>Created wireframe for College Portal UI</li>
            <li>Reviewed peer feedback on E-commerce Dashboard</li>
            <li>Completed mobile UI prototype</li>
          </ol>
        </div>
      </section>
    </div>
  );
}

function ProfileView({ user, setUser }) {
  const [form, setForm] = useState(user);

  function handleSave(e) {
    e.preventDefault();
    setUser(form);
    alert("Profile updated (demo)");
  }

  return (
    <div className="max-w-3xl mx-auto">
      <div className="bg-white p-6 rounded-lg border shadow-sm">
        <h2 className="text-xl font-semibold mb-2">Profile</h2>
        <p className="text-sm text-slate-500 mb-4">Edit your personal details</p>

        <form onSubmit={handleSave} className="grid grid-cols-1 gap-4">
          <div>
            <label className="block text-sm font-medium">Full Name</label>
            <input value={form.name} onChange={(e) => setForm({...form, name: e.target.value})} className="mt-1 block w-full rounded-md border-slate-200" />
          </div>

          <div>
            <label className="block text-sm font-medium">Course</label>
            <input value={form.course} onChange={(e) => setForm({...form, course: e.target.value})} className="mt-1 block w-full rounded-md border-slate-200" />
          </div>

          <div>
            <label className="block text-sm font-medium">Email</label>
            <input value={form.email} onChange={(e) => setForm({...form, email: e.target.value})} type="email" className="mt-1 block w-full rounded-md border-slate-200" />
          </div>

          <div className="flex gap-3">
            <button type="submit" className="px-4 py-2 rounded-md bg-indigo-600 text-white">Save</button>
            <button type="button" onClick={() => setForm(user)} className="px-4 py-2 rounded-md border">Reset</button>
          </div>
        </form>
      </div>
    </div>
  );
}

function SettingsView() {
  const [prefs, setPrefs] = useState({ notifications: true, compactMode: false, theme: "light" });

  return (
    <div className="max-w-3xl mx-auto">
      <div className="bg-white p-6 rounded-lg border shadow-sm">
        <h2 className="text-xl font-semibold mb-2">Settings</h2>
        <p className="text-sm text-slate-500 mb-4">Customize your app preferences</p>

        <div className="space-y-4">
          <div className="flex items-center justify-between">
            <div>
              <div className="text-sm font-medium">Notifications</div>
              <div className="text-xs text-slate-500">Email and app notifications</div>
            </div>
            <input type="checkbox" checked={prefs.notifications} onChange={() => setPrefs({...prefs, notifications: !prefs.notifications})} />
          </div>

          <div className="flex items-center justify-between">
            <div>
              <div className="text-sm font-medium">Compact Mode</div>
              <div className="text-xs text-slate-500">Denser lists and smaller paddings</div>
            </div>
            <input type="checkbox" checked={prefs.compactMode} onChange={() => setPrefs({...prefs, compactMode: !prefs.compactMode})} />
          </div>

          <div>
            <label className="block text-sm font-medium">Theme</label>
            <select value={prefs.theme} onChange={(e) => setPrefs({...prefs, theme: e.target.value})} className="mt-1 rounded-md border-slate-200">
              <option value="light">Light</option>
              <option value="dark">Dark</option>
            </select>
          </div>

          <div>
            <button className="px-4 py-2 rounded-md bg-indigo-600 text-white">Save Preferences</button>
          </div>
        </div>
      </div>
    </div>
  );
}
